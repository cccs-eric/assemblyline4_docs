[comment]: # (AUTOGENERATED MARKDOWN CONTENT. UPDATES TO ODM DOCUMENTATION SHOULD BE DONE THROUGH ASSEMBLYLINE-BASE REPO!)
# File
> Model of File

| Field | Type | Description | Required | Default |
| :--- | :--- | :--- | :--- | :--- |
| archive_ts | Date | Archiving timestamp | :material-checkbox-marked-outline: Yes | `None` |
| ascii | Keyword | Dotted ASCII representation of the first 64 bytes of the file | :material-checkbox-marked-outline: Yes | `None` |
| classification | Classification | Classification of the file | :material-checkbox-marked-outline: Yes | `None` |
| entropy | Float | Entropy of the file | :material-checkbox-marked-outline: Yes | `None` |
| expiry_ts | Date | Expiry timestamp | :material-minus-box-outline: Optional | `None` |
| is_section_image | Boolean | Is this an image from an Image Result Section? | :material-checkbox-marked-outline: Yes | `False` |
| hex | Keyword | Hex dump of the first 64 bytes of the file | :material-checkbox-marked-outline: Yes | `None` |
| md5 | MD5 | MD5 of the file | :material-checkbox-marked-outline: Yes | `None` |
| magic | Keyword | Output from libmagic related to the file | :material-checkbox-marked-outline: Yes | `None` |
| mime | Keyword | MIME type of the file as identified by libmagic | :material-minus-box-outline: Optional | `None` |
| seen | [Seen](/assemblyline4_docs/odm/models/file/#seen) | Details about when the file was seen | :material-checkbox-marked-outline: Yes | See [Seen](/assemblyline4_docs/odm/models/file/#seen) for more details. |
| sha1 | SHA1 | SHA1 hash of the file | :material-checkbox-marked-outline: Yes | `None` |
| sha256 | SHA256 | SHA256 hash of the file | :material-checkbox-marked-outline: Yes | `None` |
| size | Integer | Size of the file in bytes | :material-checkbox-marked-outline: Yes | `None` |
| ssdeep | SSDeepHash | SSDEEP hash of the file | :material-checkbox-marked-outline: Yes | `None` |
| type | Keyword | Type of file as identified by Assemblyline | :material-checkbox-marked-outline: Yes | `None` |


[comment]: # (AUTOGENERATED MARKDOWN CONTENT. UPDATES TO ODM DOCUMENTATION SHOULD BE DONE THROUGH ASSEMBLYLINE-BASE REPO!)
## Seen
> File Seen Model

| Field | Type | Description | Required | Default |
| :--- | :--- | :--- | :--- | :--- |
| count | Integer | How many times have we seen this file? | :material-checkbox-marked-outline: Yes | `1` |
| first | Date | First seen timestamp | :material-checkbox-marked-outline: Yes | `NOW` |
| last | Date | Last seen timestamp | :material-checkbox-marked-outline: Yes | `NOW` |

