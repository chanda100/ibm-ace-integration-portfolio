       [ T r i g g e r   S o u r c e s ]
       +-------------------------------+

       |  REST Client (OpenAPI 3) |
	   |  HTTPS Client(Authentication services implemented using Policy Project))
       |  Enterprise App (IBM MQ)      |
       
       +---------------+---------------+
                       |
                       v [JSON / XML / DFDL Payload]
+--------------------------------------------------------------+

|                IBM ACE INTEGRATION SERVER                    |
|                                                              |
|  +--------------------+      +----------------------------+  |
|  | Input Node         | ---> | LocalEnvironment & MQMD    |  |
|  | (HTTP / MQ)|      | (Context Extraction)       |  |
|  +--------------------+      +------------+---------------+  |
|                                           |                  |
|                                           v                  |
|  +--------------------+      +----------------------------+  |
|  | Graphical Data Map | <--- | Compute Node (ESQL)        |  |
|  | (.map Translation) |      | (Validation / Routing)     |  |
|  +----------+---------+      +------------+---------------+  |
|             |                             |                  |
|             +--------------+--------------+                  |
|                            |                                 |
|                            v [Transformed Logic Tree]        |
|  +--------------------------------------------------------+  |
|  | TryCatch Node                                          |  |
|  |  +--------------------------------------------------+  |  |
|  |  | Reusable Global Error Subflow (.subflow)        |  |  |
|  |  | - Parses $ExceptionList                          |  |  |
|  |  | - Logs to MQ Queue(Later from which we can log it to the database or file and forward alert(Email) to operations)                                |  |  |

|  |  +--------------------------------------------------+  |  |
|  +-------------------------+------------------------------+  |
|                            |                                 |
|                            v [Target Safe Payload]           |
|  +--------------------------------------------------------+  |
|  | Output / Request Node                                  |  |
|  | (HTTP Reply / MQOutput)                |  |
|  +-------------------------+------------------------------+  |
|                            |                                 |
+----------------------------|---------------------------------+

## 🛠️ Key Implementation Highlights Shown Here
* **RESTful Services:** Implemented clean OpenAPI 3.0 interfaces utilizing strict JSON domain parsing.
* **Advanced ESQL:** Coded complex message tree mutations and environmental context caching.
* **Global Error Framework:** Built reusable sub flows parsing the technical `$ExceptionList` tree into human-readable operations alerts.
* **Implemented authentication and transport security using Policy projects
