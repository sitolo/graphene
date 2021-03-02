Relay
~~~~~

Relay is a client-side library to interact with a GraphQL server. In order
for Relay to function, it requires a GraphQL server to follow (more specifically,
the schema which it exposes) a few conventions. These are:

1. The way that the server provides pagination
2. Uniformity around the definition of mutations
3. The way that the server handles re-fetching of data

In the first case, Relay requires that lists of data be specified as
connection objects. Connections comprise at least two fields, pageInfo,
and edges. PageInfo itself is an object that contains information for
Relay to be able to flexibly paginate over your data - backwards or forwards.
