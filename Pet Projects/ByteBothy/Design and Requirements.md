### Summary of the feature
Create a file hosting service which would list and retrieve files from specified hosted storage(S3/NAS etc.).
Given a file user should be able to upload to a specified path and retrieve the file with given path.

### Components
1. Backend service exposing APIs to upload/download/delete files.
2. Frontend service for file browser.

### Backend Service Implementation
- Language - Java
- Framework - Spring
- Database - Postgres
- Service expectations:
	- **Sign up/Login functionality**
	- **Agnostic to storage type** - Service should be agnostic to storage and should be able to detect storage based on account preference set. User would need to specify which storage they want to browse for, once the storage type is selected respective file tree would be shown.
	- **User Multi Storage options** - User can have multiple storage options attached to their account.
	- **Optimised List object implementation for S3** - Since S3 list operations are chargeable we would need to optimise it. (Might cache paths for optimisation)

### Database
- PostgresSQL
- Database: ByteBothy
**users**

|     ID     |   Int    | Primary Key |
| :--------: | :------: | ----------- |
|    name    |   text   | NOT NULL    |
|   email    |   text   | NOT NULL    |
| is_active  | boolean  | NOT NULL    |
| created_at | datetime | NOT NULL    |
| update_at  | datetime | NOT NULL    |
|  password  |   text   | NOT NULL    |
**storage_types**

|     ID     |   int    |          |
| :--------: | :------: | -------- |
|    name    |   text   | NOT NULL |
| created_at | datetime | NOT NULL |
| update_at  | datetime | NOT NULL |

**user_storage_preferences**

|    userId    |   int    | foreign_key(user_id) NOT NULL       |
| :----------: | :------: | ----------------------------------- |
| storage_type |   int    | foreign_key(storage_types) NOT NULL |
|  created_at  | datetime | NOT NULL                            |
|  update_at   | datetime | NOT NULL                            |
**aws_account_credentials** - Table to store credentials for S3.

|    ID     | int | Primary Key                         |
| :-------: | :-: | ----------------------------------- |
| accountId | int | foreign_key(a) NULL                 |
| password  | int | NOT NULL                            |
| username  | int | foreign_key(storage_types) NOT NULL |

**aws_accounts**

|      ID       |   int    | Primary Key                          |
| :-----------: | :------: | ------------------------------------ |
|    userId     |   int    | foreign_key(user_id)                 |
| credential_id |   int    | foreign_key(aws_account_credentials) |
|  created_at   | datetime | NOT NULL                             |
|   update_at   | datetime | NOT NULL                             |

**nas


