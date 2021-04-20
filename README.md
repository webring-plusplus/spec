`webring++` is a logical extension of 1990s [webring](https://en.wikipedia.org/wiki/Webring).

This spec provides a language-agnostic explanation of how to join a `webring++`.

## How to join a `webring++`

In a traditional webring, websites in the collection form a linked list.
Inserting into a linked list requires:

1. Creating a new node `n` with back pointer to `n - 1` and forward pointer to `n + 1`
2. Adjusting `n - 1`'s forward pointer to be `n`
3. Adjusting `n + 1`'s back pointer to be `n`

In a `webring++`, there is no ring at all - we instead abstract the idea to a [digraph](https://en.wikipedia.org/wiki/Directed_graph).
Each site in the `webring++` points to zero or more other sites.
As such, to join an existing `webring++`, you just need one of the other nodes to add your site to its forward set.

## Technical details

`webring++` only specifies forward links.
`webring++` clients can choose how to read, render, walk, or otherwise ignore members of the digraph.

To host forward links in `webring++`, you must host an endpoint that serves JSON matching the schema indicated below.
`webring++` endpoints are scoped by hostport, which means that the following are all the same:

- `a.com:8080/webring++.json`
- `https://a.com/webring++.json`

The following are not valid, and are ignored:

- `a.com/blog/webring++.json`
- `geocities.blog/blakeh/webring++.json`

The following are all valid, but fall into different hostports and so do not collide (they exist in parallel):

- `blog.coolwebshit.com/webring++.json`
- `forums.coolwebshit.com/webring++.json`
- `anything.else.really.coolwebshit.com/webring++.json`

### Schema

The top-level `/webring++.json` endpoint should serve JSON with the following schema:

Key       | Description
---       | ---
`version` | Number. Valid value is `0` or `1`.
`links` | List of Strings. Each string should be a URI which represents a forward link. Anything after [`port`](https://en.wikipedia.org/wiki/Uniform_Resource_Identifier#Syntax) should be ignored by the client.

**Clients must permit deserialisation of unknown object keys for forward-compatibility**.
