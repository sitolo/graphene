Graphene Internals
~~~~~~~~~~~~~~~~~~

Since the official GraphQL documentation follows a schema-first approach and most resources are
oriented around this, it may be difficult to understand how Graphene works internally. This
guide provides context on the flow of operations in Graphene from creating a schema, executing a query
against it, and having the data you've requested returned back to you.

Beforehand, it's important to note that Graphene follows a code-first development
approach, meaning that your implementation drives the definition of the GraphQL schema,
but uses GraphQL-core internally, which follows a schema-first approach. GraphQL-core itself expects a schema definition, which
it parses into Python-specific classes in order to run. Instead, Graphene implements a set
of primitives that maps to the aforementioned classes, circumnavigating the need to define
a schema completely. Looking at it this way, the purpose of Graphene is to provide a way to map to GraphQL-core which is ultimately
responsible for handling your requests. In other words, when you use Graphene, you are writing your
schema as Python code.

It is able to do this by providing a set of types to you which map to GraphQL-core types at runtime. This is equivalent
to saying that Graphene provides a set of Python classes which correspond to those made available on the SDL.

At compile-type, Graphene scans the root types you've provided as arguments to the schema, creates a type map
from them, and then feeds this into GraphQL-core. The type map specifies the equivalent GraphQL-core type
for each Graphene type. GraphQL-core is then run.

Execution Resolution
--------------------

GraphQL uses resolvers to determine how to fetch your data.

Consider the following definition of a Shirt comprising of three fields,
color, brand, and size:

.. code-block::

    class Shirt(ObjectType):
        color = String()
        brand = String()
        size = Int()

The equivalent definition of this object using the SDL would be:

.. code-block::

    type Shirt {
        color: Int
        brand: String
        size: Int
    }

Graphene knows how to return the specific Shirt you've requested
using resolvers which given a request for a field, fills that field
with data. Resolvers chain together up until a scalar value is reached.

Schema
------

A GraphQL schema is a strongly-typed definition of your GraphQL server - that is, the data and operations
that you permit your clients to perform, written in the `Schema Definition Language`_, commonly referred to as the SDL.
The SDL simply consists of a set of types which contain fields and operations on those types.

.. _Schema Definition Language: https://graphql.org/learn/schema/

Creating a schema results in the creation of a **GraphQLSchema** which consists
of a combination of root queries, mutations, subscriptions, directives, and
additional types that you have specified. Each of these root types are
used to create the type system which maps the schema to the underlying
data source - a **GraphQLTypeMap**. Simply put, they begin the resolution
chain until the what you ask for gets resolved.



Since Graphene is built on top of graphql-core, it uses this
to execute operations against the schema.
