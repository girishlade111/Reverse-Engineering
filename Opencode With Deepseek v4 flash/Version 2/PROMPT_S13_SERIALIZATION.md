========================================================================
SUPPLEMENTARY PROMPT S13: SERIALIZATION
========================================================================
Supplementary Analysis: Serialization, Marshaling, and Data Formats
Enterprise Reverse Engineering Prompt Framework

========================================================================
PURPOSE
========================================================================

This supplementary prompt analyzes serialization/deserialization
patterns, data format handling, encoding/decoding, and data
contract management.

========================================================================
WHEN TO USE
========================================================================

Execute this prompt if:
- Multiple data formats are used (JSON, XML, Protobuf, etc.)
- Custom serialization/deserialization logic exists
- Protocol Buffers, Avro, Thrift, or similar are used
- Data validation during deserialization is critical
- Serialization versioning exists

Execute after Phase 4.

========================================================================
ACTIVITIES
========================================================================

S13.1. SERIALIZATION FRAMEWORK ANALYSIS
- Document serialization libraries and versions
- Document format converters/adapters
- Document custom serialization logic
- Document serialization configuration

S13.2. SCHEMA CONTRACT ANALYSIS
- Document all data contracts/schemas
- Document schema evolution strategy
- Document backward/forward compatibility rules
- Document schema registry usage
- Document field deprecation handling

S13.3. ENCODING/DECODING PIPELINES
- Document encoding/decoding chains
- Document encryption during serialization
- Document compression during serialization
- Document validation during deserialization
- Document error handling during parsing

S13.4. BINARY FORMAT ANALYSIS (if applicable)
- Document binary format structure
- Document bit/byte layout
- Document endianness and alignment
- Document version/header signatures

========================================================================
REQUIRED ARTIFACTS
========================================================================

ARTIFACT S13.1: SERIALIZATION_FRAMEWORKS.md
ARTIFACT S13.2: SCHEMA_CONTRACTS.md
ARTIFACT S13.3: ENCODING_PIPELINES.md
ARTIFACT S13.4: BINARY_FORMATS.md (if applicable)

========================================================================
END OF PROMPT S13
========================================================================
