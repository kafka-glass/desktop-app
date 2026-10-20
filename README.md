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

## 📦 Quick Start (Pre-built Release)

If you just want to use the application without compiling from source:

1. **Install `librdkafka`** via [Homebrew](https://brew.sh/):
   ```bash
   brew install librdkafka
   ```

2. **Download the latest release**:
   - Go to [Releases](https://code.rbi.tech/IUAD16AS/raiffeisen-kafka/releases) and download **`KafkaGlassExplorer-macOS.zip`**.
   - Unzip the archive and move **`KafkaGlassExplorer.app`** into your **`/Applications`** folder.

3. **First launch (macOS Gatekeeper bypass)**:
   Since this internal enterprise application is distributed without a paid Apple Developer certificate, macOS Gatekeeper blocks it on the first launch. Run this command in Terminal once to allow it to run:
   ```bash
   xattr -cr /Applications/KafkaGlassExplorer.app
   ```
   *Alternative: In Finder, open `/Applications`, right-click (or Control-click) `KafkaGlassExplorer.app`, click **Open**, and confirm **Open** in the dialog.*

---

## 🛠️ Prerequisites (For Building from Source)

- **macOS**: Sonoma (14.0) or later
- **Swift**: 5.10 or later / Xcode 15 or later
- **librdkafka**: Required C library provider

Install `librdkafka` via [Homebrew](https://brew.sh/):

```bash
brew install librdkafka
```

---

## 🚀 Building & Running

### Using Swift Package Manager (CLI)

1. Clone or navigate to the repository directory:
   ```bash
   cd raiffeisen-kafka
   ```

2. Build the project:
   ```bash
   swift build
   ```

3. Run the unit & integration tests:
   ```bash
   swift test
   ```

4. Launch the application:
   ```bash
   swift run KafkaGlassExplorer
   ```

### Using Xcode

Open `Package.swift` directly in Xcode:

```bash
open Package.swift
```

Select the `KafkaGlassExplorer` executable scheme and click **Run** (`⌘R`).

### 🚢 Publishing a Release (Maintainers)

An automated release script is provided in [`scripts/release.sh`](scripts/release.sh). It compiles a Release binary, creates the `.app` bundle, configures dynamic linker rpaths for `librdkafka` (supporting both Apple Silicon and Intel), applies ad-hoc codesigning, compresses to zip, and publishes directly to GitHub Releases via GitHub CLI (`gh`):

```bash
# Publish release using version from Info.plist
./scripts/release.sh

# Or publish a specific version tag
./scripts/release.sh v1.0.1
```

---

## 🏗️ Project Architecture

```
Sources/
├── CRdKafka/                        # C module map header bridging librdkafka
│   └── module.modulemap
└── KafkaGlassExplorer/
    ├── App/
    │   └── KafkaGlassExplorerApp.swift  # App entry point, AppDelegate & Window styling
    ├── Models/
    │   ├── ClusterProfile.swift         # SwiftData schema for cluster configs
    │   └── KafkaModels.swift            # Models for topics, partitions, groups, records
    ├── Services/
    │   ├── KafkaEngine.swift            # High-performance async wrapper around librdkafka
    │   ├── SchemaRegistryService.swift  # Confluent Schema Registry HTTP client
    │   ├── MessageDeserializer.swift    # Avro, JSON, XML, Text, Hex decoders
    │   └── KeychainService.swift        # Secure credential storage
    ├── ViewModels/
    │   └── AppState.swift               # Observable app state & connection coordinator
    ├── Views/
    │   ├── MainView.swift               # Root NavigationSplitView layout
    │   ├── SidebarView.swift            # Cluster selector, topic tree & consumer groups
    │   ├── TopicDetailContainerView.swift # Properties, Data, Partitions, Config tabs
    │   ├── TopicDataView.swift          # Message fetcher controls & records table
    │   ├── MessageInspectorView.swift   # Detailed payload & record key inspector
    │   └── ClusterSettingsModalView.swift # Add/Edit cluster profile modal
    └── UI/
        └── LiquidGlassTheme.swift       # Visual effect cards, search bar, design tokens
Tests/
└── KafkaGlassExplorerTests/
    ├── KafkaGlassExplorerTests.swift    # Avro, Wire format & XML deserialization tests
    └── KafkaEngineIntegrationTests.swift # Broker communication & topic fetching tests
```

---

## 📄 License

This project is intended for internal enterprise development and Kafka exploratory workflows.
