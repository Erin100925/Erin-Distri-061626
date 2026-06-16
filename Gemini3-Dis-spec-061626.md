Technical Specification: WOW Medical Distribution Studio
Project Version: 2.0.1 (Studio Edition)
Classification: Enterprise Supply Chain Analytics & Medical Regulatory Compliance
Document Type: Full-System Technical Design Specification
1. Executive Summary
The WOW Medical Distribution Studio is a sophisticated full-stack application designed to solve the critical challenges of medical device traceability, regulatory compliance (specifically Taiwan's FDA/CDC standards), and supply chain optimization. By integrating high-performance data standardization logic with Google’s Gemini Generative AI, the studio transforms raw, fragmented distribution logs into actionable intelligence.
The system provides a "Command Center" interface that allows supply chain officers to ingest multi-source CSV/TXT data, standardize it via canonical auto-mapping, and execute advanced analytical "Magic" agents to detect anomalies, optimize logistics, and predict safety stock requirements.
2. Technological Ecosystem
2.1 Frontend Architecture
Framework: React 19 (Single Page Application)
Build Tool: Vite 6 with HMR disabled for agent stability.
Styling: Tailwind CSS 4 utilizing the new @import "tailwindcss"; engine.
Animation Engine: Framer Motion (motion/react) for spatial transitions and liquid layout shifts.
State Management: React Hooks (useMemo, useRef, useState) for localized high-frequency updates.
Visualization: Recharts for temporal trends and hierarchical data; custom SVG/CSS-in-JS for network topologies.
Icons: Lucide-React for a consistent semantic design language.
2.2 Backend Architecture
Server: Node.js Express (v4.21.2).
Language: TypeScript (Strict mode).
Runtime: tsx for high-speed dev execution.
Security: dotenv for environment isolation and server-side Gemini key proxying.
2.3 AI Orchestration
SDK: @google/genai (v2.4.0).
Model Tier: Primary: gemini-3.1-flash-lite (optimized for latency); Secondary: gemini-1.5-pro (complex auditing).
Pipeline Architecture: Multi-step visualizer simulating "tokenization -> routing -> processing -> inference -> formatting."
3. Data Engineering & The "Canonical Standardizer"
A core pillar of the studio is its ability to handle "dirty data" from disparate sources (Supplier A and Customer B).
3.1 Data Schema (The StandardRecord)
The application enforces a rigid TypeScript interface for every distribution record to ensure interoperability:
code
TypeScript
interface StandardRecord {
  id: string;          // GUID with source prefix
  supplier_id: string; // Uniform ID (e.g., B00160)
  customer_id: string; // Uniform ID (hospital handles)
  deliver_date: string;// ISO8601 (YYYY-MM-DD)
  license_no: string;  // Regulatory index
  category: string;    // Medical classification
  udi_di: string;      // Unique Device Identifier
  device_name: string; // Normalized string
  lot_no: string;      // Batch tracking
  serial_no: string;   // Individual unit traceability
  model: string;       // Model variant
  quantity: number;    // Int quantity
  expiry_date: string; // Critical for safety recalls
  status: "Verified" | "Manual Map" | "Investigate";
}
3.2 Standardization Heuristics
The /src/data.ts module contains advanced regex and positional mapping logic:
Fuzzy Date Parsing: Converts 2026/05/15, 20260515, and 2026-05-15 into a canonical format.
Typography Correction: Automatically strips Chinese "smart quotes" (“ and ”) that often break CSV parsers.
Positional vs. Labeled Mapping: The system detects if a dataset is a "Supplier Raw" format (position-based) or a "Hospital Declaration" format (header-based) and applies different extraction matrices.
4. Visual Identity & Design Philosophy
The application utilizes Psychological Color Theory and Pantone Branding to reduce cognitive load in high-stress compliance environments.
4.1 Pantone-Driven Theming
Instead of generic HEX codes, the UI implements 10 specific Pantone styles (e.g., Classic Blue 19-4052, Viva Magenta 18-1750). These styles govern:
Glassmorphism: Translucent panels with backdrop-blur-md and bg-slate-900/60.
Dynamic Gradients: The background utilizes a multi-stop radial gradient that shifts based on the selected Pantone, creating a sense of "Living Data."
Status Glows: Components utilize custom glowClass properties (e.g., shadow-[0_0_15px_rgba(15,76,129,0.5)]) to highlight active status.
5. Functional Command Pillars
5.1 Interactive Analytics Dashboard
Temporal Rhythm Heatmap: A weekly grid showing delivery density, allowing officers to identify weekend bottlenecks.
Network Topology Graph: A custom-rendered map linking Suppliers → Licenses → Buyers. It uses useMemo for performance and identifies high-risk "Single Link" dependencies.
Pareto Concentration: Recharts-driven analysis to identify the "Top 20%" of suppliers responsible for 80% of volume.
5.2 The AI Magic Workspace
This module acts as a "Head Compliance Officer" by allowing the user to trigger pre-configured prompts:
Trace Anomaly Auditor: Scans for SN/Lot collisions (duplicate items in different hospitals).
Distribution Optimizer: Suggests re-routing for redundant shipping hops.
Hazard Recall Tracker: Specifically scans "Class III" implants for expiry risks.
6. Three Additional "WOW" AI Features (The Future of the Studio)
To extend the capability of the design, the following three architectural features are proposed:
6.1 Feature 1: Autonomous Regulatory Sentinel (Digital Twin Sync)
Concept: A background AI agent that maintains a "Digital Twin" of the regulatory environment.
Technical Implementation: The Gemini engine is configured to periodically "hallucinate" or (via future APIs) scrape government license databases. If a license in the dataset is flagged as "Expired" or "Under Inquiry" in the external authority system, the Studio triggers a Class-Red visual alert across all records associated with that License No.
User Value: Proactive compliance. Instead of finding errors after an audit, the system flags them the moment data is ingested.
6.2 Feature 2: Geo-Epidemiological Demand Correlation
Concept: Cross-referencing distribution volume with regional health data.
Technical Implementation: By integrating (simulated) regional hospital admission data, Gemini performs a cross-correlation analysis. For example, if Pacific-Coast hospitals show a 10% rise in cardiac admissions, the AI suggests a Safety Stock Increase for Pacemakers (B00160) in that specific geo-cluster before the shortage occurs.
User Value: Predictive logistics that save lives by ensuring high-risk implants are physically located where patient demand is spiking.
6.3 Feature 3: Semantic Supply-Chain Conflict Resolver
Concept: Removing the "Naming Babel" problem.
Technical Implementation: When the system detects high-similarity device names with different IDs (e.g., "Pacemaker v1" vs "Cardiac Pacer Gen 1"), the Gemini Semantic Agent runs a clustering algorithm to "Repair" the names into a single Canonical Product Identity.
User Value: Clean, consolidated data displays even when five different suppliers use five different naming conventions for the exact same product.
7. Infrastructure & Security Framework
7.1 Server-Side Proxying
To protect the GEMINI_API_KEY, all AI requests are routed through /api/gemini. The frontend never sees the secret. The server enforces a 50mb JSON limit to allow for large document analysis while maintaining stability.
7.2 Lazy Initialization
The getGeminiClient function uses a singleton pattern to initialize the AI client only when needed. This ensures the application remains functional even if API keys are missing during the initial setup phase.
8. 20 Comprehensive Follow-up Questions
To further refine the evolution of this Studio, consider the following technical and strategic queries:
Performance: How does the Recharts rendering engine handle >10,000 records in a single browser session?
Security: Is the current express.json({ limit: "50mb" }) setting sufficient for large multi-year audits, or should we move to a streaming CSV parser?
Scalability: Should we transition the local records state to a persistent Cloud SQL instance for enterprise-grade persistence?
UX: Would a "Focus Mode" that hides the AI Workspace improve efficiency for pure data-entry tasks?
Integration: Can we implement an automated SFTP listener that imports new distribution files every midnight?
Compliance: Does the system meet the ISO-13485 standards for computerized system validation?
AI Accuracy: What is the "Hallucination Buffer" when Gemini summarizes thousands of medical SNs?
Internationalization: Should we add support for Spanish or Japanese for global medical distributors?
Visualization: Would a D3.js Forced-Directed Graph provide better clarity for complex network topologies than the current Recharts implementation?
Regulatory: How can we automate the export into the specific Taiwan FDA (TFDA) XML format for direct government filing?
Inventory: Can the "Safety Stock" formula be customized per-hospital, rather than applied globally?
Mobile: Given the density of the dashboard, should we build a mirrored "Alerts Only" mobile view?
Auth: When moving to production, should we implement Firebase Auth for per-user audit trails?
Error Handling: How does the system handle "Invalid UTF-8" characters common in legacy medical ERP exports?
Telemetry: Should we add a real-time "Health Pulse" to the server to monitor API latencies?
AI Models: Would gemini-2.0-flash offer a significant speed improvement for the "Magic" tasks?
Data Privacy: How do we ensure PII (Patient Identifiable Information) is scrubbed before being sent to the LLM?
Testing: Should we implement Playwright end-to-end tests to verify the "Standardization" logic against 100+ edge-case CSVs?
Export: Would a "PDF Report Generator" that creates a branded compliance certificate be a priority?
Offline: For sites with poor connectivity, can we integrate IndexedDB for local-first data persistence?
