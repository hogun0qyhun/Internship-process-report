# PostgreSql

PostgreSQL là một hệ thống cơ sở dữ liệu quan hệ đối tượng mã nguồn mở mạnh mẽ sử dụng và mở rộng ngôn ngữ SQL kết hợp với nhiều tính năng lưu trữ và mở rộng an toàn các khối lượng công việc dữ liệu phức tạp nhất. Nguồn gốc của PostgreSQL có từ năm 1986 như một phần của dự án POSTGRES tại Đại học California tại Berkeley và có hơn 35 năm phát triển tích cực trên nền tảng cốt lõi.

PostgreSQL đã tạo dựng được danh tiếng vững chắc nhờ kiến ​​trúc đã được chứng minh, độ tin cậy, tính toàn vẹn của dữ liệu, bộ tính năng mạnh mẽ, khả năng mở rộng và sự tận tụy của cộng đồng nguồn mở đằng sau phần mềm để liên tục cung cấp các giải pháp hiệu suất cao và sáng tạo. PostgreSQL chạy trên tất cả các hệ điều hành chính , tuân thủ ACID kể từ năm 2001 và có các tiện ích bổ sung mạnh mẽ như trình mở rộng cơ sở dữ liệu không gian địa lý PostGIS phổ biến . Không có gì ngạc nhiên khi PostgreSQL đã trở thành cơ sở dữ liệu quan hệ nguồn mở được nhiều người và tổ chức lựa chọn.

___
## Tính năng có trong PostgreSQL

PostgreSQL đi kèm với nhiều tính năng nhằm giúp các nhà phát triển xây dựng ứng dụng, quản trị viên bảo vệ tính toàn vẹn của dữ liệu và xây dựng môi trường chịu lỗi, đồng thời giúp bạn quản lý dữ liệu của mình bất kể tập dữ liệu lớn hay nhỏ. Ngoài việc miễn phí và mã nguồn mở , PostgreSQL còn có khả năng mở rộng cao. Ví dụ, bạn có thể định nghĩa kiểu dữ liệu của riêng mình, xây dựng các hàm tùy chỉnh, thậm chí viết mã từ các ngôn ngữ lập trình khác nhau mà không cần biên dịch lại cơ sở dữ liệu của bạn!

PostgreSQL cố gắng tuân thủ tiêu chuẩn SQL khi sự tuân thủ đó không mâu thuẫn với các tính năng truyền thống hoặc có thể dẫn đến các quyết định kém về mặt kiến ​​trúc. Nhiều tính năng theo yêu cầu của tiêu chuẩn SQL được hỗ trợ, mặc dù đôi khi có cú pháp hoặc chức năng hơi khác nhau. Có thể mong đợi các động thái tuân thủ tiếp theo theo thời gian. Kể từ bản phát hành phiên bản 16 vào tháng 9 năm 2023, PostgreSQL tuân thủ ít nhất 170 trong số 177 tính năng bắt buộc để tuân thủ SQL:2023 Core. Tính đến thời điểm viết bài này, không có cơ sở dữ liệu quan hệ nào đáp ứng đầy đủ sự tuân thủ với tiêu chuẩn này.

- Data Types
  * Primitives: Integer, Numeric, String, Boolean
  * Structured: Date/Time, Array, Range / Multirange, UUID
  * Document: JSON/JSONB, XML, Key-value (Hstore)
  * Geometry: Point, Line, Circle, Polygon
  * Customizations: Composite, Custom Types
- Data Integrity
   * UNIQUE, NOT NULL
   * Primary Keys
   * Foreign Keys
   * Exclusion Constraints
   * Explicit Locks, Advisory Locks
- Concurrency, Performance
   * Indexing: B-tree, Multicolumn, Expressions, Partial
   * Advanced Indexing: GiST, SP-Gist, KNN Gist, GIN, BRIN, Covering indexes, Bloom filters
   * Sophisticated query planner / optimizer, index-only scans, multicolumn statistics
   * Transactions, Nested Transactions (via savepoints)
   * Multi-Version concurrency Control (MVCC)
   * Parallelization of read queries and building B-tree indexes
   * Table partitioning
   * All transaction isolation levels defined in the SQL standard, including Serializable
   * Just-in-time (JIT) compilation of expressions
- Reliability, Disaster Recovery
   * Write-ahead Logging (WAL)
   * Replication: Asynchronous, Synchronous, Logical
   * Point-in-time-recovery (PITR), active standbys
   * Tablespaces
- Security
   * Authentication: GSSAPI, SSPI, LDAP, SCRAM-SHA-256, Certificate, and more
   * Robust access-control system
   * Column and row-level security
   * Multi-factor authentication with certificates and an additional method
- Extensibility
   * Stored functions and procedures
   * Procedural Languages: PL/pgSQL, Perl, Python, and Tcl. There are other languages available through extensions, e.g. Java, JavaScript (V8), R, Lua, and Rust
   * SQL/JSON constructors, query functions, path expressions, and JSON_TABLE
   * Foreign data wrappers: connect to other databases or streams with a standard SQL interface
   * Customizable storage interface for tables
   * Many extensions that provide additional functionality, including PostGIS
- Internationalisation, Text Search
   * Support for international character sets, e.g. through ICU collations
   * Case-insensitive and accent-insensitive collations
   * Full-text search

Ngoài ra, PostgreSQL có khả năng mở rộng cao: nhiều tính năng, chẳng hạn như chỉ mục, có API được xác định để bạn có thể xây dựng với PostgreSQL để giải quyết các thách thức của mình.

PostgreSQL đã được chứng minh là có khả năng mở rộng cao cả về số lượng dữ liệu mà nó có thể quản lý và số lượng người dùng đồng thời mà nó có thể đáp ứng. Có các cụm PostgreSQL đang hoạt động trong môi trường sản xuất quản lý nhiều terabyte dữ liệu và các hệ thống chuyên biệt quản lý petabyte.
















