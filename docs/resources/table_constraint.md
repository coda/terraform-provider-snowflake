---
# generated by https://github.com/hashicorp/terraform-plugin-docs
page_title: "snowflake_table_constraint Resource - terraform-provider-snowflake"
subcategory: ""
description: |-
  
---

# snowflake_table_constraint (Resource)



## Example Usage

```terraform
resource "snowflake_database" "d" {
  name = "some_db"
}

resource "snowflake_schema" "s" {
  name      = "some_schema"
  database  = snowflake_database.d.name
}

resource "snowflake_table" "t" {
  database            = snowflake_database.d.name
  schema              = snowflake_schema.s.name
  name                = "some_table"

  column {
    name     = "col1"
    type     = "text"
    nullable = false
  }

   column {
    name     = "col2"
    type     = "text"
    nullable = false
  }
}

resource "snowflake_table" "fk_t" {
  database            = snowflake_database.d.name
  schema              = snowflake_schema.s.name
  name                = "fk_table"

  column {
    name     = "fk_col1"
    type     = "text"
    nullable = false
  }

   column {
    name     = "fk_col2"
    type     = "text"
    nullable = false
  }
}

resource "snowflake_table_constraint" "primary_key" {
  name="myconstraint"
  type="PRIMARY KEY"
  table_id = snowflake_table.t.id
  columns = ["col1"]
  comment = "hello world"
}

resource "snowflake_table_constraint" "foreign_key" {
  name="myconstraintfk"
  type="FOREIGN KEY"
  table_id = snowflake_table.t.id
  columns = ["col2"]
  foreign_key_properties {
    references {
      table_id = snowflake_table.fk_t.id
      columns = ["fk_col1"]
    }
  }
  enforced = false
  deferrable = false
  initially = "IMMEDIATE"
  comment = "hello fk"
}
```

<!-- schema generated by tfplugindocs -->
## Schema

### Required

- `columns` (List of String) Columns to use in constraint key
- `name` (String) Name of constraint
- `table_id` (String) Idenfifier for table to create constraint on. Must be of the form Note: format must follow: "<db_name>"."<schema_name>"."<table_name>" or "<db_name>.<schema_name>.<table_name>" or "<db_name>|<schema_name>.<table_name>" (snowflake_table.my_table.id)
- `type` (String) Type of constraint

### Optional

- `comment` (String) Comment for the table constraint
- `deferrable` (Boolean) Whether the constraint is deferrable
- `enable` (Boolean) Specifies whether the constraint is enabled or disabled. These properties are provided for compatibility with Oracle.
- `enforced` (Boolean) Whether the constraint is enforced
- `foreign_key_properties` (Block List, Max: 1) Additional properties when type is set to foreign key. Not applicable for primary/unique keys (see [below for nested schema](#nestedblock--foreign_key_properties))
- `initially` (String) Whether the constraint is initially deferred or immediate
- `rely` (Boolean) Specifies whether a constraint in NOVALIDATE mode is taken into account during query rewrite.
- `validate` (Boolean) Specifies whether to validate existing data on the table when a constraint is created. Only used in conjunction with the ENABLE property.

### Read-Only

- `id` (String) The ID of this resource.

<a id="nestedblock--foreign_key_properties"></a>
### Nested Schema for `foreign_key_properties`

Optional:

- `match` (String) The match type for the foreign key. Not applicable for primary/unique keys
- `on_delete` (String) Specifies the action performed when the primary/unique key for the foreign key is deleted. Not applicable for primary/unique keys
- `on_update` (String) Specifies the action performed when the primary/unique key for the foreign key is updated. Not applicable for primary/unique keys
- `references` (Block List, Max: 1) The table and columns that the foreign key references. Not applicable for primary/unique keys (see [below for nested schema](#nestedblock--foreign_key_properties--references))

<a id="nestedblock--foreign_key_properties--references"></a>
### Nested Schema for `foreign_key_properties.references`

Required:

- `columns` (List of String) Columns to use in foreign key reference
- `table_id` (String) Name of constraint

## Import

Import is supported using the following syntax:

```shell
terraform import snowflake_table_constraint.example 'myconstraintfk❄️FOREIGN KEY❄️test|test|table'
```