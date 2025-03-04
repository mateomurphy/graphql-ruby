---
layout: guide
doc_stub: false
search: true
enterprise: true
section: GraphQL Enterprise - Changesets
title: Releasing Changesets
desc: Associating changes to version numbers
index: 3
---

To be available to clients, Changesets added to the schema with `use GraphQL::Enterprise::Changeset::Release`:

```ruby
class MyAppSchema < GraphQL::Schema
  use GraphQL::Enterprise::Changeset::Release, changesets: [
    Changesets::DeprecateRecipeFlag,
    Changesets::RemoveRecipeFlag,
  ]
end
```

Only changesets on the list will be shown to clients. The `release ...` configuration in the changeset will be compared to `context[:changeset_version]` to determine if the changeset applies to the current request.

## Inspecting Releases

To preview releases, you can create schema dumps by passing `context: { changeset_version: ... }` to {{ "Schema.to_definition" | api_doc }}.

For example, to see how the schema looks with `API-Version: 2021-06-01`:

```ruby
schema_sdl = MyAppSchema.to_definition(context: { changeset_version: "2021-06-01"})
# The GraphQL schema definition for the schema at version "2021-06-01":
puts schema_sdl
```

To make sure schema versions don't change unexpectedly, use the techniques described in the {% internal_link "Schema structure guide", "/testing/schema_structure" %}.
