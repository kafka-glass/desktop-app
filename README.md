# KafkaGlass Explorer

<img width="2048" height="2048" alt="Kafka-Glass" src="https://github.com/user-attachments/assets/ffa6a167-72e0-41a0-a504-3bb96eceba6a" />

**KafkaGlass Explorer** is a high-performance, native macOS desktop application designed for inspecting, monitoring, and debugging Apache Kafka clusters. Built with Swift 5.10+ and SwiftUI using modern Liquid Glass design aesthetics, it provides a fast, lightweight, and developer-friendly alternative to bloated web-based Kafka tools.

---

## ✨ Features

### 🖥️ Native macOS Experience
- **Liquid Glass Design**: Elegant macOS interface utilizing ultra-thin materials, smooth blur transitions, and responsive typography.
- **Split Navigation**: Intuitive `NavigationSplitView` architecture with seamless cluster and topic management.
- **Integrated Traffic Controls**: Optimized macOS window chrome and sidebar navigation.

### 🏢 Multi-Cluster & Security Management
- **Cluster Profiles**: Configure and quickly switch between local, DEV, QA, and PROD clusters.
- **SwiftData & Keychain Storage**: Connection profiles persisted via SwiftData; credentials (SASL passwords, private key passphrases, Schema Registry secrets) securely stored in the macOS Keychain.
- **Enterprise Security Support**:
  - `PLAINTEXT`
  - `SSL` (Mutual TLS / mTLS with client certificate & CA verification)
  - `SASL_PLAINTEXT` & `SASL_SSL` (Mechanisms: `PLAIN`, `SCRAM-SHA-256`, `SCRAM-SHA-512`, `GSSAPI/Kerberos`)
- **Confluent Schema Registry Integration**: Connects with Schema Registry instances for dynamic Avro decoding.

### 📊 Topics & Consumer Groups
- **Topic Hierarchy & Outlines**: Hierarchical topic list showing partition counts and live message statistics.
- **Real-Time Topic Metrics**:
  - Partition distribution and message distribution progress bars
  - Replication factor and cleanup policy
  - Leader broker, replicas, ISR (In-Sync Replicas), earliest/latest offsets
- **Topic Configuration**: Complete breakdown of topic-level configs (`retention.ms`, `cleanup.policy`, `min.insync.replicas`, etc.).
- **Consumer Group Overview**: View active consumer groups, group states (`Stable`, `Empty`, `Dead`), and partition assignments.

### 🔍 Advanced Message Fetching & Filtering
- **Fetch Modes**:
  - **Newest (Tail)**: Quick tail inspection of the latest messages
  - **Earliest**: Read from the start of the topic/partition
  - **Specific Offset**: Seek directly to a chosen offset
  - **By Timestamp**: Jump to records produced around a specific date and time
- **Client-Side Search**: Instant in-memory filtering by record key across fetched records.

### 🔬 Intelligent Message Inspector
- **Full-Width Payload Viewer**: Code and payload display with text selection and fast copy.
- **Multi-Format Deserializer**:
  - **Auto Detect**: Automatically identifies Confluent Avro, JSON, XML, or plain text.
  - **Avro (Parsed)**: Decodes Confluent Wire Format (Magic Byte `0x00` + Schema ID) using schemas fetched dynamically from Schema Registry.
  - **JSON**: Formatted with pretty-printing and sorted keys.
  - **XML**: Formatted and indented via native `XMLDocument`.
  - **Plain Text**: Clean UTF-8 string rendering.
  - **Hex View**: Canonical byte offset, hex dump, and ASCII representation.
- **Key Inspection**: Dedicated Record Key display with one-click copy support.

---

## 📄 License

This project is intended for internal enterprise development and Kafka exploratory workflows.
