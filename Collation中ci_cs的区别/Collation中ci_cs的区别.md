# collation 

MySQL collation defines the rules for how strings are compared and sorted. Each collation is associated with a character set and defines rules for character comparison, including case sensitivity, accent sensitivity, and other locale-specific behavior. Here’s an overview of key aspects of MySQL collations:

1. **Character Set and Collation:**
   - **Character Set:** Defines the set of symbols and encodings (like `utf8mb4`, `latin1`, etc.). It's the encoding for storing text data in the database.
   - **Collation:** Defines the rules for comparing characters in a particular character set. It controls how string comparison is performed, considering case sensitivity, accent marks, and other locale-specific rules.
2. **Case Sensitivity:**
   - **Case-Insensitive (CI):** Treats letters as equal regardless of case. For example, 'A' equals 'a'.
   - **Case-Sensitive (CS):** Differentiates between uppercase and lowercase letters. For example, 'A' is not equal to 'a'.
   - **Binary (BIN):** Compares strings based on the binary representation of characters, which is case-sensitive and accent-sensitive.
3. **Accent Sensitivity:**
   - Some collations also consider accent marks in comparisons. For example, in an accent-sensitive collation, 'á' and 'a' are different.
4. **Examples of Collations:**
   - `utf8mb4_general_ci`: Case-insensitive for UTF-8 multibyte characters.
   - `utf8mb4_bin`: Binary collation for UTF-8 multibyte characters, case-sensitive and accent-sensitive.
   - `latin1_swedish_ci`: Case-insensitive for Latin1 characters, following Swedish sorting rules.
   - `utf8mb4_unicode_ci`: Case-insensitive, following the Unicode standard for comparison.
5. **Choosing the Right Collation:**
   - The choice depends on your specific needs. For example, if you need case-sensitive comparisons, choose a case-sensitive or binary collation. If you're handling multilingual data, prefer a Unicode collation like `utf8mb4_unicode_ci`.
6. **Changing Collation:**
   - You can set the collation at the server, database, table, or column level. To change it for a column, you can use the `ALTER TABLE` statement, as mentioned earlier.
7. **Locale-Specific Collations:**
   - MySQL also offers collations tailored to specific languages and locales, which consider the local sorting and comparison rules.

Understanding and selecting the correct collation for your data and query requirements is crucial for achieving the desired behavior in string comparisons and sorting operations in MySQL.

# collation performance 

The performance difference between case-insensitive (CI) and case-sensitive (CS) collations in MySQL is generally minimal and depends on the specific context and workload. Here’s a comparison:

- **Case-Insensitive (CI) Collation:**
  - In a case-insensitive collation, MySQL does not differentiate between uppercase and lowercase letters. This can be slightly faster in some situations because MySQL can ignore case differences when comparing strings.
  - Index lookups can be quicker in some cases because different case variations of the same string are treated as equal, potentially reducing the number of unique index entries.
- **Case-Sensitive (CS) Collation:**
  - In a case-sensitive collation, MySQL distinguishes between uppercase and lowercase letters. This means that 'A' and 'a' are considered different values.
  - It might be slightly faster for operations that involve binary comparisons because it can compare raw byte values directly without needing additional logic to handle case conversion or sensitivity.
- **Other Considerations:**
  - The actual performance impact of using CI vs. CS collation is usually small and may not be noticeable in many applications.
  - The choice between CI and CS should be based more on the application's data requirements (e.g., whether 'a' should be considered equal to 'A') rather than on performance.
  - The performance can also be influenced by other factors, such as the size of the dataset, the complexity of the queries, the use of indexes, and the specific database configuration.

In summary, while there can be slight performance differences between case-sensitive and case-insensitive collations, these differences are usually minimal. The choice should primarily be based on the functional requirements of your application regarding how it should treat case variations in string data.

# example

```sql


CREATE TABLE `word` (
  `en` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NOT NULL,
  `cn` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NOT NULL,
  UNIQUE KEY `uk_en` (`en`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;

INSERT INTO `word` (`en`, `cn`) VALUES ('who', '谁');
-- 由于大小写不敏感，所以会插不进去
INSERT INTO `word` (`en`, `cn`) VALUES ('WHO', '世界卫生组织');



```



