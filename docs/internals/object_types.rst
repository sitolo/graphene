
Object Types
~~~~~~~~~~~~

Most succiently, an Object Type represents a single type in a GraphQL schema.

When you write:

.. code-block::

    class Foo(ObjectType):
        bar = String()
        baz = String()

This corresponds to the following block in your schema:

.. code-block::

    type Foo {
        bar: String
        baz: String
    }

Graphene uses the concept of mounted and unmounted types to enable
you to express your types using the name of the field as opposed to
having to write something like:

.. code-block::

    class Foo(ObjectType):
        bar = Field(String)

in order to reduce repetition of the Field object. Internally,
String inherits from UnmountedType, and provides its type which
is used to map to the field.
