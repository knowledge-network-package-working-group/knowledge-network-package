## Content and Media Assets
### Content
Nodes have `content`. This field holds the node's information as JSON. Rather than enforcing a strict schema, it's up to the system administrator to set up types that suit the organization's needs. This has a couple of advantages: 
* We don't need to adhere to a universal structure; all applications are different, and so they need different types of models. 
* We retain control over the type's properties and styling (how they are displayed on screen), which are then stored to ensure consistent behavior between environments.

At the end, all valid JSON is valid in `content`. However, applications should always check for the node's type, which contain the `content_schema` and `content_font` fields necessary for schema validation and correct node displaying. More details on [Index Database Schema](index-database-schema.md).

#### Properties
Edges can have `properties`. Even when an edge's label can be descriptive enough of a relationship between two nodes, sometimes extra information is required. The same rules from the previously described `content` field apply here too, where `properties_schema` defines the fields necessary for `properties`.

### Media Assets
When a node is inserted into the index database, one can specify a dedicated directory to be automatically created for its assets. A node's `content` field can then reference any asset within its own directory using a relative path.

The dedicated asset directory is reserved for the node's binary objects (blobs), and the directory's path is generated using a SHA256 hash from the node's `id`.

![Entity relationship diagram of how a node's contents and assets are stored](/content/assets/er_diagram_content.png)

For example, given we have a node with an `id` of 1824, its generated SHA256 hash will be: `2ced184d8477465987593807f31360e94b539aa41f515e0a973179f881663698`. 

Then, we place the asset directory under the first two characters of this hash as parent directories (this is called "sharding"), resulting in the path: `/assets/2/c/2ced184d8477465987593807f31360e94b539aa41f515e0a973179f881663698`.

Blobs contained inside this path can then be called by the node's `content` field. Ultimately, to retrieve these assets, we only need the node's `id` and the asset's filename.
