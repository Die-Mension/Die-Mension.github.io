# JOBBOSS MASTER REFERENCE MANUAL
## Die-Mension LLC — Brunswick, OH
### Precision Tool & Die Manufacturing

---

**Document Purpose:** Definitive reference guide for JobBoss/JobBOSS² configuration and operation at Die-Mension. Enables Karen Mitchell (office manager) and any AI assistant to configure, troubleshoot, and operate JobBoss effectively — especially the unconfigured Material Control module.

**Version:** 1.0  
**Generated:** 2026-02-25  
**Coverage:** JobBoss on-premise (legacy) and JobBOSS² cloud (ECI Solutions)  

**Confidence Tags Used Throughout:**
- `[CONFIRMED]` — Verified from official ECI documentation or direct experience
- `[LIKELY]` — High-confidence inference from multiple consistent sources
- `[UNVERIFIED]` — Plausible based on partial information; verify before acting
- `[SPECULATIVE]` — Educated guess; low confidence; requires verification

---

## TABLE OF CONTENTS

1. [Product Overview & History](#section-1-product-overview--history)
2. [Complete Module List](#section-2-complete-module-list)
3. [Material Control Deep Dive](#section-3-material-control-deep-dive) ← **PRIORITY: Unconfigured at Die-Mension**
4. [Lot & Serial Tracking Deep Dive](#section-4-lot--serial-tracking-deep-dive)
5. [Data Collection (Shop Floor) Deep Dive](#section-5-data-collection-shop-floor-deep-dive)
6. [Database & Technical Architecture](#section-6-database--technical-architecture)
7. [API & Integration Guide](#section-7-api--integration-guide)
8. [Reporting Guide](#section-8-reporting-guide)
9. [System Administration & Configuration](#section-9-system-administration--configuration)
10. [Training Resources](#section-10-training-resources)
11. [Common Problems & Solutions](#section-11-common-problems--solutions)
12. [Glossary](#section-12-glossary)
13. [Quick Reference for AI Assistants](#section-13-quick-reference-for-ai-assistants)
14. [Appendix: User Reviews & Community Feedback](#appendix-user-reviews--community-feedback)

---



---

No matching skill applies. Proceeding to write the two sections in full.

---

# SECTION 1: PRODUCT OVERVIEW & HISTORY

## 1.1 Product Identity and Naming History

JobBOSS has undergone several rebranding and ownership transitions since its inception. Understanding this history is important when reading legacy documentation, searching support forums, or communicating with ECI support staff, as terminology varies depending on the era of the resource.

### Naming Timeline

| Era | Product Name | Owner | Notes |
|-----|-------------|-------|-------|
| Pre-2017 | JobBoss | Exact Software / Shoptech Industrial Software | Original on-premise Windows application; SQL Server backend |
| 2017 | JobBoss | ECI Software Solutions | ECI acquires Shoptech; product name and functionality largely unchanged |
| 2020 | JobBoss / E2 Shop System | ECI Software Solutions | ECI acquires Shoptech's cloud-native E2 Shop System to gain modern cloud architecture |
| 2021–Present | JobBOSS² (cloud) / JobBoss (on-premise) | ECI Software Solutions | Cloud product rebranded as "JobBOSS²" (note the superscript 2); legacy on-premise product retains the "JobBoss" or "JobBoss²" label depending on version |

### Key Points for AI Assistants and Administrators

- The product may appear as "JobBoss," "Job Boss," "JobBOSS," "JobBOSS²," or "JB2" in various contexts. These all refer to the same ECI product family.
- The cloud product (JobBOSS²) is architecturally distinct from the legacy on-premise version. Advice from pre-2020 documentation may not apply to JobBOSS² and vice versa.
- ECI also owns and maintains several other manufacturing ERP products in its portfolio, including **E2 Shop System**, **M1 ERP**, **Macola**, **Traverse**, and **JobBOSS** itself. Do not confuse E2 Shop System (now marketed independently as a simpler alternative) with JobBOSS².
- "Shoptech" is a historical company name. If any installer files, registry keys, or directory paths reference "Shoptech," they belong to the legacy on-premise product.

---

## 1.2 Product Tiers and Deployment

### Subscription Tiers

JobBOSS² is offered in three subscription tiers. The tier determines which modules are available. When configuring the system or troubleshooting missing features, the first diagnostic question should be: **"What tier is this account subscribed to?"**

| Tier | Target Shop Size | Key Inclusions | Key Exclusions |
|------|-----------------|----------------|----------------|
| **Silver** | 1–10 users, very small shops | Estimating, Order Entry, Job Management, basic Purchasing, basic Inventory, AR/AP, basic Reporting | Advanced Scheduling, Quality, Payroll, KnowledgeSync, uniPoint |
| **Gold** | 5–30 users, growing shops | All Silver features plus: Scheduling (Planning Board), Shop Floor Data Collection, Receiving, full Inventory, Shipping | uniPoint Quality, some advanced analytics |
| **Platinum** | 10–150 users, full-featured | All Gold features plus: advanced Scheduling (Whiteboard, Advisor), uniPoint Quality integration, full Reporting suite, Dashboards & KPIs, Payroll, all integrations |

> **Note for Die-Mension:** Confirm the current tier with ECI before attempting to configure any module. Attempting to access a module not included in the subscribed tier will either show no menu option or display an access-denied message.

### Deployment Models

| Deployment | Product Name | Hosting | Database Access | Update Control |
|------------|-------------|---------|----------------|----------------|
| **Cloud** | JobBOSS² | ECI-managed (AWS or Azure) | No direct SQL access; API/reporting only | ECI pushes updates; shop has limited control over version |
| **On-Premise** | JobBoss / JobBoss² | Shop-managed Windows Server | Full SQL Server access | Shop controls update timing via ECI installer |

### Pricing Reference (Approximate, as of 2024–2025)

- **Subscription:** Starting at approximately $200 per user per month (cloud); varies by tier and user count
- **Implementation:** Typically $5,000 to $15,000+ depending on data migration complexity, number of modules, and amount of ECI professional services engaged
- **Add-ons:** KnowledgeSync, uniPoint Quality, and Alora Machine Intelligence are separately licensed and priced

> **Common Mistake:** Assuming that the on-premise license fee structure matches the cloud subscription. They are billed differently. Legacy on-premise customers may be on older perpetual-license plus annual maintenance models.

---

## 1.3 ECI Company Overview

### Company Profile

| Field | Detail |
|-------|--------|
| Full Company Name | ECI Software Solutions |
| Headquarters | Austin, TX, USA |
| Ownership | Private equity-backed (Leonard Green & Partners as of recent reporting) |
| Market Focus | Small-to-medium manufacturers, job shops, field service, building supply |
| Website | [ecisolutions.com](https://www.ecisolutions.com) |
| Product Website | [jobboss.com](https://www.jobboss.com) |
| Customer Portal | [customerportal.ecisolutions.com](https://customerportal.ecisolutions.com) |

### Contact Numbers

| Purpose | Phone Number |
|---------|-------------|
| General Sales | 800-525-2143 |
| Alternate General | 800-677-9640 |
| Technical Support | (866) 342-8392 |

### Support Resources

- **Customer Portal:** The primary self-service hub at `customerportal.ecisolutions.com`. Used for submitting support tickets, accessing product documentation, downloading software updates (on-premise), and tracking ticket status.
- **Ideas Portal:** Community-driven feature request voting system. Die-Mension should monitor this portal for permission-related improvements (see Section 2, Module 25) and submit votes on relevant ideas. Key known Ideas Portal submissions referenced in this manual are tracked by JBCORE-I prefixed IDs.
- **Knowledge Base:** Available through the customer portal; contains articles, release notes, and how-to guides. Search using both old and new product name variants.
- **Community Forums:** ECI hosts a user community where shop owners and administrators share workarounds. Valuable for edge cases not addressed in official documentation.

### ECI Product Portfolio (for Context)

ECI owns multiple ERP products. When seeking support or documentation, verify that the resource you are reading pertains to JobBOSS/JobBOSS² and not another ECI product.

| ECI Product | Target Audience | Notes |
|-------------|----------------|-------|
| JobBOSS² | Job shops, precision manufacturers | This manual's subject |
| E2 Shop System | Simple job shops, very small operations | Simpler UI; cloud-native; distinct from JobBOSS² |
| M1 ERP | Mid-market manufacturers | More complex than JobBOSS; separate support team |
| Macola | Distribution, process manufacturing | Unrelated to job shop workflow |
| Traverse | Accounting-focused manufacturers | General business accounting ERP |

---

## 1.4 Technology Stack

### Backend Database

| Component | Detail |
|-----------|--------|
| Database Engine | Microsoft SQL Server (2016, 2019, or 2022 depending on version) |
| Default Database Name | `JobBoss32` (legacy on-premise) or `production` (some installations) |
| Access Method (Cloud) | No direct SQL access; data available via Makini API or Crystal/Stimulsoft reports |
| Access Method (On-Premise) | Full SQL Server access via SSMS or ODBC connection |
| Schema Notes | Highly normalized relational schema; key tables include `Job`, `Job_Operation`, `Employee`, `Customer`, `Vendor`, `Material`, `Purchase_Order`, `Packing_List`, `Invoice` |

> **Important for AI Assistants:** When a user asks for a SQL query against JobBoss, assume the on-premise deployment unless told otherwise. For cloud deployments, SQL queries must be run through the reporting layer or the Makini API. Direct SQL queries against the ECI-hosted cloud database are not permitted.

### Reporting Infrastructure

| Component | Applies To | Notes |
|-----------|-----------|-------|
| Crystal Reports | Legacy on-premise JobBoss | Report files use `.rpt` extension; requires Crystal Reports runtime on the server/workstation |
| Stimulsoft | JobBOSS² cloud | Embedded in the web UI; supports web-based report designer; export to PDF, Excel, Word |
| User Defined Reports | Both | Custom report builder within JobBoss UI; limited compared to Crystal/Stimulsoft but accessible to non-developers |

### Mobile Applications (iOS)

| App | App Store ID | Purpose |
|-----|-------------|---------|
| JobBOSS² Data Collection App | 1213396029 | Shop floor clock-in/out, job scanning, barcode-based labor entry |
| JobBOSS² Employee DC App | 1287589083 | Employee-facing data collection; lighter UI than full app |

> **Note:** Android support has been limited historically. Verify current Android availability with ECI support before purchasing Android-based shop floor terminals.

### Integration Middleware and Platforms

| Platform | Role | Licensed Separately |
|----------|------|-------------------|
| KnowledgeSync ITC | Alerting and workflow automation engine; monitors SQL for conditions and sends email/SMS notifications | Yes |
| Makini.io | REST API platform; provides 30+ standardized manufacturing data entities for integration with third-party systems | Yes |
| QuickBooks (Desktop & Online) | Native integration for financial data sync | Included (connector); QuickBooks license separate |
| CADLink (QBuild Corporation) | CAD-to-JobBoss connectivity for BOM and part data | Third-party; separate license |
| Alora Machine Intelligence | Machine monitoring and OEE data platform; integrates machine utilization into JobBoss job costing | Third-party; separate license |

### Cloud Infrastructure

- ECI hosts JobBOSS² on either **Amazon Web Services (AWS)** or **Microsoft Azure**, depending on the data center region and contract era.
- ECI manages all infrastructure including backups, patching, and uptime.
- Shops do not receive direct access to cloud infrastructure. All data access is through the web UI, mobile apps, or the Makini API.
- SLA terms are defined in the ECI Master Service Agreement (MSA) signed at time of purchase.

---

## 1.5 Ideal Customer Profile

### Fit Assessment

JobBOSS/JobBOSS² is purpose-built for a specific type of manufacturer. Understanding this profile helps Die-Mension benchmark their use of the system and anticipate limitations when workflows deviate from the intended use case.

| Factor | Ideal Fit | Poor Fit |
|--------|-----------|----------|
| Employee Count | 1–50 (sweet spot); 51–150 (adequate) | 150+ (growing pains; consider M1 or larger ERP) |
| Manufacturing Type | High-mix, low-volume, custom/make-to-order | High-volume repetitive manufacturing, process manufacturing |
| Shop Type | Job shop, contract machine shop, metal fabricator, precision parts manufacturer, tool & die | Automotive assembly, food processing, pharmaceutical, distribution |
| Order Pattern | Unique or semi-repeat jobs with custom routings | Mass production with fixed routings and high volumes |
| Quality Requirements | ISO 9001, AS9100 with uniPoint add-on | IATF 16949 (automotive-specific; limited support) |
| Financial Complexity | Simple job costing with QuickBooks integration | Multi-entity, multi-currency, complex consolidations |

### Die-Mension Specific Notes

Die-Mension (Brunswick, OH) is a small precision tool and die shop — this profile is the canonical ideal customer for JobBOSS². The system is designed to handle:

- Highly custom, low-volume tooling and die work
- Complex routing structures with multiple operations per job
- Precise job costing against quoted estimates
- Vendor-managed outside processing (heat treating, plating, grinding)
- Customer-facing documentation (travelers, packing slips, COCs)

Where Die-Mension's workflows deviate from standard job shop patterns (e.g., blanket orders for repeat tooling, multi-release deliveries, inventory of standard stock materials), specific configuration guidance is provided in the relevant module sections of this manual.

---

# SECTION 2: COMPLETE MODULE LIST AND OVERVIEW

## Introduction to the Module Structure

JobBOSS² is organized into discrete functional modules, each mapped to a business process area. Modules share underlying data (jobs, customers, vendors, materials, employees) but have distinct screens, access controls, and workflows. Some modules are interdependent and must be configured in a specific sequence. This section documents each module in detail, including prerequisites, common workflows, and known limitations.

> **For AI Assistants:** When a user asks how to perform a task in JobBOSS, first identify which module owns that task using this section, then refer to the module's key screens and workflow entry points to provide accurate guidance. Always verify prerequisites before advising configuration steps.

---

## 2.1 Estimating & Quoting

### What It Does

The Estimating & Quoting module is the origination point for all production work at a job shop. It allows sales and estimating staff to build detailed cost estimates for custom parts, including labor (by operation and work center), materials, outside processing, and markup. Approved estimates become quotes sent to customers, and accepted quotes convert directly into sales orders with a single action.

### Key Screens and Features

| Screen / Feature | Description |
|-----------------|-------------|
| Quote Header | Customer selection, quote number, salesperson, due date, revision level, notes |
| Line Item Detail | Part number, quantity, description, unit price, lead time |
| Estimate Detail | Bill of operations (routing), material costs, outside processing costs, burden rates, labor rates — all pulled from Table Maintenance defaults |
| Material BOM Tab | Attach required materials with quantities; ties to Inventory if stock is on hand |
| Outside Processing Tab | List vendor services (e.g., heat treat, plating) with estimated costs |
| Markup & Pricing | Configure markup by category; system calculates total cost and suggested selling price |
| Quote Templates | Save frequently quoted part configurations as templates to speed re-quoting |
| AI BOM Builder | Import BOM from uploaded PDFs, Excel files, images, or CSVs; AI parses and maps to JobBoss material and operation fields |
| Estimated vs. Actual | After a job completes, compare quoted cost to actual job cost in reports |
| One-Click Conversion | Convert an accepted quote to a sales order without re-entering data |

### Who Uses It

- **Estimators:** Primary users; build detailed cost estimates
- **Sales Staff:** Review quotes, send to customers, track quote status
- **Shop Managers:** Review routing assumptions and labor/burden rates in estimates

### Dependencies and Prerequisites

Before using this module, you must:

1. Configure **Work Centers** in Table Maintenance (Section 2.24) — labor rates and burden rates are pulled from work center records
2. Set up **Operations** in Table Maintenance — standard operations (turning, milling, grinding, etc.) form the routing basis
3. Configure **Material Types** and **Units of Measure** in Table Maintenance
4. Set up **Customers** with pricing tiers and terms
5. Define **Markup Tables** if using category-based markup (configured in Table Maintenance > Markup Codes)

### Common Workflow Entry Points

1. Receive RFQ from customer (phone, email, portal)
2. Create new Quote in Estimating module
3. Select customer; enter part description and quantity
4. Build routing (bill of operations) — select operations, assign work centers, enter estimated hours
5. Add materials from inventory or create new material record
6. Add outside processing vendors and estimated costs
7. Review system-calculated cost; apply markup; set selling price
8. Print or email quote to customer
9. Upon customer approval: one-click convert to Sales Order

### Limitations

- **AI BOM Builder accuracy:** The AI parsing of PDFs and images is probabilistic. Always review AI-generated BOMs before committing to a quote. Complex or hand-drawn drawings may parse inaccurately.
- **Revision control on quotes:** JobBoss tracks quote revisions, but linking quote revision history to part revision history requires manual discipline.
- **Multi-currency:** Limited or no multi-currency support in the base product. If Die-Mension quotes international customers in non-USD currencies, pricing conversion must be handled manually.
- **Quote expiration:** Quote expiration dates are informational only; the system does not auto-expire or lock quotes.

---

## 2.2 Order Entry / Sales Orders

### What It Does

The Order Entry module manages the conversion of customer requirements into active sales orders (SOs). It stores customer pricing agreements, manages blanket orders with multiple release schedules, and serves as the link between customer commitments and production planning. Sales orders drive the creation of work orders (jobs).

### Key Screens and Features

| Screen / Feature | Description |
|-----------------|-------------|
| Sales Order Header | Customer, SO number, order date, requested date, salesperson, shipping terms, PO number |
| Line Items | Part, quantity ordered, unit price, required date per line |
| Blanket Order Management | Configure a single SO with multiple release dates and quantities |
| Job Split Function | Split a blanket order release into discrete production jobs, each with its own traveler and scheduled dates |
| Customer Pricing Storage | Stored pricing by customer/part combination; auto-populates on new orders |
| Terms and Credit | Payment terms, credit limit check, tax exemption status |
| Order Acknowledgment | Print or email order acknowledgment to customer |
| Job Creation from SO | One-click or auto-creation of work orders linked to SO line items |

### Who Uses It

- **Sales Staff:** Enter new orders, manage customer relationships, track order status
- **Customer Service:** Handle order changes, delivery inquiries, blanket order releases
- **Production Planning:** Review open orders to determine production priorities

### Dependencies and Prerequisites

Before using this module, you must:

1. Complete **Customer Setup** — full customer record including shipping address, billing address, payment terms, tax status
2. Configure **Estimating** (Section 2.1) if converting quotes to orders
3. Set up **Salesperson records** in Table Maintenance if tracking by salesperson

### Common Workflow Entry Points

**Path 1 — From Quote:**
1. Open accepted quote in Estimating
2. Click "Convert to Order"
3. Verify SO header data; confirm pricing
4. Save SO; system prompts to create job(s)

**Path 2 — Direct Order Entry:**
1. Navigate to Order Entry > New Sales Order
2. Select customer; enter customer PO number
3. Add line items (part number, quantity, price, due date)
4. Save; create linked work order(s)

**Path 3 — Blanket Order:**
1. Create SO with blanket order flag enabled
2. Define total blanket quantity and contract period
3. Add release schedule: date and quantity per release
4. As each release date approaches, use Job Split to generate discrete work orders

### Limitations

- **Job Split complexity:** For blanket orders with many releases, managing Job Split manually can be time-consuming. No automated release triggering based on date.
- **Sales order change control:** Modifying a sales order after a job has been created does not automatically update the job. Job must be manually revised.
- **No built-in customer portal:** Customers cannot view order status directly. Status updates must be communicated manually or via a third-party integration.

---

## 2.3 Job Management / Work Orders

### What It Does

The Job Management module is the central production document in JobBOSS. Every piece of work performed in the shop is tracked through a job (also called a work order). Jobs accumulate all costs — labor, material, outside processing — and serve as the basis for job costing, scheduling, shipping, and invoicing. The job traveler (a printed document that travels with the part through the shop) is generated from this module.

### Key Screens and Features

| Screen / Feature | Description |
|-----------------|-------------|
| Job Header | Job number, customer, part number, quantity ordered, quantity complete, due date, job type, status |
| Routing / Operations Tab | Sequence of operations; each operation has work center, estimated hours, actual hours, labor cost |
| Material Tab | Materials required; can be allocated from inventory or flagged for purchase |
| Outside Processing Tab | Vendor services in the routing; generates subcontract POs |
| Notes / Documents Tab | Attach drawings, specs, customer notes; internal shop notes |
| Job Traveler | Printable document with job details, routing steps, material list, quality checkpoints |
| Job Status | Open, In Process, Complete, On Hold, Cancelled — tracks workflow state |
| Sub-Jobs | Child jobs linked to a parent job; used for assemblies or multi-component orders |
| Split Jobs | Divide a job quantity into parallel production streams |
| Revisions | Track changes to the job after production begins; maintain revision history |
| Time & Material Jobs | Alternative job type where costs accumulate and are billed at actuals |
| Assembly Jobs | Jobs that consume other jobs (sub-jobs) as components |

### Job Types

| Type | Description | Use Case at Die-Mension |
|------|-------------|------------------------|
| Standard | Fixed quantity, estimated costs, invoice at completion | Most tooling and die work |
| Assembly | Parent job that consumes sub-jobs | Complex dies with multiple components |
| Time & Material | Costs accumulate; billed at actual | Repair work, warranty, emergency service |

### Who Uses It

- **Production Managers:** Create and manage jobs; monitor status
- **Shop Supervisors:** Review travelers; assign work to operators
- **Shop Floor Operators:** Reference travelers; report labor via Data Collection
- **Accounting:** Use job cost data for invoicing and financial reporting

### Dependencies and Prerequisites

Before using this module, you must:

1. Configure **Work Centers** (Section 2.24) — every operation must have an assigned work center
2. Configure **Operations** in Table Maintenance — define standard operations with default times and rates
3. Set up **Employees** in Table Maintenance with pay rates and skills
4. Configure **Materials** in Inventory (Section 2.8) if allocating stock
5. Link to a **Sales Order** if order-driven (recommended); standalone jobs are also supported

### Common Workflow Entry Points

1. Job created automatically from Sales Order conversion, OR manually via Job Management > New Job
2. Enter routing (operations sequence); assign work centers and estimated hours
3. Attach required materials; flag for purchase or allocate from stock
4. Add outside processing steps; system will generate subcontract POs upon job release
5. Print job traveler; release job to shop floor
6. Operators report labor via Data Collection (Section 2.5) or manual time entry
7. Upon quantity completion: mark job Complete; trigger Shipping (Section 2.6)
8. Job costs roll up to Accounts Receivable for invoicing (Section 2.10)

### Limitations

- **Job number format:** If using Shop Floor Data Collection with barcode scanning, job numbers must be **numeric only** (no alpha characters, no special characters). This is a hard system constraint that affects job numbering convention for the entire shop.
- **Retroactive changes:** Modifying a job's routing after labor has been posted against operations is possible but can create costing discrepancies. Establish a change control procedure.
- **No native CAD attachment viewer:** Drawings attached to jobs are stored as file links; viewing requires the workstation to have the appropriate CAD viewer installed.
- **Sub-job complexity:** Deeply nested sub-job structures (more than 2–3 levels) can become difficult to manage and report on.

> **Common Mistake:** Creating jobs without linking them to a Sales Order. Unlinked jobs do not appear on order-based reports and cannot be invoiced through the standard AR workflow without additional steps.

---

## 2.4 Scheduling

### What It Does

The Scheduling module provides tools to plan and manage production capacity across all work centers. It allows schedulers to view current workload, identify capacity bottlenecks, resequence jobs, and communicate schedules to the shop floor. JobBOSS offers both finite and infinite capacity scheduling modes.

### Key Screens and Features

| Screen / Feature | Description |
|-----------------|-------------|
| Planning Board | Drag-and-drop Gantt chart; visualize all jobs across all work centers by day/week/month |
| Whiteboard Scheduler | Real-time schedule adjustment interface; designed for shop floor display or scheduler workstation |
| Scheduling Advisor | Identifies at-risk jobs (behind schedule, past due, insufficient capacity); prioritization tool |
| Capacity View | View available vs. consumed hours by work center, day, week, or month |
| Finite Scheduling | Schedules jobs within actual capacity limits; prevents overbooking |
| Infinite Scheduling | Schedules without capacity constraints; useful for planning before committing |
| Backward Scheduling | Schedules from due date backward; ensures on-time delivery |
| Forward Scheduling | Schedules from today forward; shows earliest possible completion |
| Machine/Employee Scheduling | Schedule by machine (work center) or by individual employee |
| Schedule Lock | Lock a schedule to prevent automatic rescheduling from disrupting committed sequences |

### Who Uses It

- **Schedulers:** Primary users; manage daily and weekly scheduling decisions
- **Shop Managers:** Use Scheduling Advisor for exception management; review capacity
- **Sales Staff:** Use capacity data to set realistic due dates on new quotes and orders

### Dependencies and Prerequisites

Before using this module, you must:

1. Configure **Work Centers** in Table Maintenance with:
   - Available hours per day/week
   - Efficiency percentage
   - Number of machines/operators
   - Calendar (work days, holidays, shifts)
2. All **Jobs** must have operations assigned with estimated hours and work center assignments
3. **Operations** must have standard times configured for meaningful scheduling output
4. **Employee calendars** must be set up if scheduling by employee

> **Before using this module, you must** complete Work Center setup in Table Maintenance. Scheduling with incomplete or inaccurate work center capacity data produces unreliable results and should not be used for customer commitment dates.

### Common Workflow Entry Points

1. Navigate to Scheduling > Planning Board
2. Review unscheduled jobs (appear in backlog queue)
3. Drag jobs onto work center timeline slots
4. Use Scheduling Advisor to identify jobs at risk of missing due date
5. Adjust sequence using drag-and-drop or right-click menu
6. Print or export schedule for shop floor distribution
7. Use Whiteboard for real-time day-of adjustments

### Limitations

- **Automatic rescheduling:** JobBOSS is not a full APS (Advanced Planning & Scheduling) system. It does not automatically reoptimize the entire schedule when disruptions occur. Manual intervention is required.
- **Setup time:** The system does not natively model sequence-dependent setup times (e.g., a setup that is shorter if run after a specific prior job).
- **Subcontract operations:** Outside processing operations appear in the routing but may not be visible on the scheduling board in a meaningful way; lead times for outside processing must be manually managed.
- **Resource calendars:** If employee vacation or machine downtime is not entered in calendars, the schedule will show false capacity.

---

## 2.5 Shop Floor Data Collection

### What It Does

Shop Floor Data Collection (SFDC) allows operators on the shop floor to report labor time against jobs and operations without leaving the production area. Time is captured via barcode scanning, touchscreen terminals, or the mobile app. Data flows directly into job costing in real time, eliminating end-of-day or end-of-week manual time entry.

### Key Screens and Features

| Screen / Feature | Description |
|-----------------|-------------|
| Collection Terminal Configuration | Set up physical or virtual terminals in Table Maintenance; each terminal has an ID and location |
| Clock In / Clock Out | Operator scans employee badge + job barcode to start/stop time on a specific operation |
| Job Barcode | Printed on the job traveler; encodes job number and operation number |
| DCMobile App | iOS app for mobile data collection; mirrors terminal functionality on a tablet or phone |
| BATCH Entry | Supervisory function to enter multiple time records in batch; useful for catch-up entry or corrections |
| Real-Time Job Cost Update | Every clock-out triggers a labor cost calculation and posts to the job record immediately |
| Employee Time Review | Supervisors can review and correct time entries prior to payroll processing |
| Outside Processing Receipt | Operators can receive outside processing operations back to a job via the terminal when parts return from a vendor |

### Who Uses It

- **Shop Floor Operators:** Clock in/out on jobs throughout the day
- **Supervisors:** Monitor active labor; correct errors via BATCH entry
- **Production Managers:** View real-time job progress based on operations clocked
- **Accounting:** Labor costs feed job costing for accurate invoicing

### Dependencies and Prerequisites

Before using this module, you must:

1. **Job numbers must be numeric only** — this is a hard system constraint for barcode scanning compatibility. Alpha characters or special characters in job numbers will prevent barcode-based clock-in.
2. Set up **Collection Terminal records** in Table Maintenance — one record per physical or virtual terminal
3. Configure **Employee records** with badge numbers if using badge scan for identification
4. Print **job travelers** with barcodes — barcodes are generated from the job number
5. Ensure shop floor devices (tablets, terminals, phones) can reach the JobBOSS server (cloud: internet connection; on-premise: local network)
6. Install the **DCMobile app** (App Store ID 1213396029) on iOS devices if using mobile collection

### Common Workflow Entry Points

1. Operator picks up job traveler at job start
2. At Collection Terminal: scan employee badge (or enter employee number)
3. Scan job barcode on traveler (or enter job number)
4. Select operation from list (or operation barcode if printed)
5. Clock IN — timer starts; system records start time and operator
6. At operation completion: return to terminal; scan badge and job again
7. Clock OUT — system calculates elapsed time; posts labor cost to job
8. Supervisor reviews at end of shift via Employee Time Review screen
9. Any corrections made via BATCH Entry

### Limitations

- **Numeric job numbers only:** This is a non-negotiable system constraint. If the shop has historically used alphanumeric job numbers (e.g., "T-2024-001"), the job numbering convention must be changed before implementing SFDC. Plan this carefully — existing open jobs with alpha numbers cannot be scanned.
- **No GPS or geofencing:** The system does not verify that an employee is physically at the correct terminal or machine.
- **Offline mode:** If the cloud connection is lost, the mobile DCMobile app may not record time. Verify offline capability with ECI before relying on it in areas with poor connectivity.
- **Overlapping time entries:** If an operator clocks into a second job without clocking out of the first, the system behavior depends on configuration. Define a shop policy and test before go-live.

> **Common Mistake:** Setting up Collection Terminals in Table Maintenance but forgetting to assign the terminal to a physical location or failing to test the barcode scanner with actual job traveler barcodes before go-live. Always run a full end-to-end test: print a traveler, scan it, verify the labor record posts to the job.

---

## 2.6 Shipping / Packing

### What It Does

The Shipping module manages the outbound delivery of completed parts to customers. It produces packing slips, shipping documents, bills of lading (BOLs), and triggers the invoicing process. Proper use of this module ensures that shipments are documented, customer records are updated, and accounting is notified to generate invoices.

### Key Screens and Features

| Screen / Feature | Description |
|-----------------|-------------|
| Packing List Creation | Select job(s) and quantities to ship; system generates packing list number |
| 4-Step Packing List Process | Step 1: Select jobs/quantities. Step 2: Verify shipping address. Step 3: Enter carrier and tracking. Step 4: Post shipment. |
| Bill of Lading (BOL) | Generate BOL for freight carriers; includes weight, piece count, carrier info |
| Packing Slip Print | Customer-facing document listing shipped items, quantities, and job/PO references |
| Certificate of Conformance (COC) | Optional document attesting that shipped parts meet customer specifications |
| Partial Shipment | Ship partial quantities against a job; job remains open for remaining quantity |
| Shipping Labels | Print address labels for packages |
| Integration with AR | Posted packing lists trigger invoice creation in Accounts Receivable |

### Who Uses It

- **Shipping Department:** Primary users; pack and document all outbound shipments
- **Accounting:** Monitors posted packing lists to trigger invoice generation

### Dependencies and Prerequisites

Before using this module, you must:

1. **Jobs must be at or near completion status** — you can only ship against jobs that have quantity completed
2. **Customer shipping address** must be verified in the customer record
3. Configure **shipping carriers** in Table Maintenance if tracking carrier-specific BOL formats
4. Understand the relationship between packing lists and invoicing: a packing list that is posted (Step 4 complete) creates an invoice record in AR

### Common Workflow Entry Points

1. Production notifies shipping that a job is complete
2. Shipper opens Shipping module > New Packing List
3. Select customer and job(s) to ship
4. Enter quantity shipped (can be partial)
5. Confirm shipping address
6. Enter carrier name and tracking number
7. Post packing list — this locks the shipment record and notifies AR
8. Print packing slip and any required certificates
9. Accounting opens AR to find the pending invoice generated by the shipment

### Limitations

- **No native carrier rate shopping:** JobBOSS does not integrate with UPS, FedEx, or USPS rate APIs. Carrier selection and rate calculation must be done outside the system.
- **COC generation:** Basic COC is generated by the system, but it is not a full quality record. For AS9100-compliant COCs with material certifications and traceability, the uniPoint add-on or manual documentation is required.
- **Packing list corrections:** Correcting a posted packing list (e.g., wrong quantity) requires creating a credit memo in AR and potentially reversing the shipment. Establish a procedure for shipping errors before go-live.

---

## 2.7 Purchasing

### What It Does

The Purchasing module manages all outbound procurement activities — buying raw materials, standard stock, and outside processing services. It supports the full purchasing lifecycle from purchase requests through PO issuance to receipt and vendor invoice matching.

### Key Screens and Features

| Screen / Feature | Description |
|-----------------|-------------|
| Purchase Order Header | Vendor, PO number, buyer, order date, required date, ship-to location |
| PO Line Items | Material or service, quantity, unit price, required date, linked job (if job-specific) |
| Standard PO | One-time purchase of materials or services |
| Blanket / Contract PO | Long-term vendor agreement with periodic releases; tracks total quantity and amount |
| Subcontract PO | Generated automatically from outside processing operations on a job; links back to job routing |
| Purchase Request | Internal request for a purchase; routed to buyer for approval and PO creation |
| RFQ Workflow | Send requests for quote to multiple vendors; compare responses; select vendor |
| Vendor Management | Vendor record: contacts, terms, lead times, services offered, approved status |
| Returns from Stock | Return unused inventory to vendor; generates return PO |
| PO Status Tracking | Open, Partial, Complete, Closed — track what has been received against each PO |

### Who Uses It

- **Purchasing Agents / Buyers:** Primary users for all procurement activity
- **Production Managers:** Trigger purchase requests for job-specific materials
- **Accounting:** Match vendor invoices to POs in Accounts Payable

### Dependencies and Prerequisites

Before using this module, you must:

1. Configure **Vendors** with full records: address, payment terms, lead times, approved status, contact names
2. Set up **Materials** in Inventory (Section 2.8) to create PO line items against stock items
3. Configure **Outside Processing** vendors if generating subcontract POs from job routings
4. Establish **approval workflows** if purchase requests require management sign-off (managed in User Administration)

### Common Workflow Entry Points

**Path 1 — Routine Material Purchase:**
1. Purchasing identifies material need (from MRP, reorder report, or request)
2. Create new PO; select vendor
3. Add line items (material, quantity, price)
4. Print or email PO to vendor
5. Receiving completes receipt against PO (Section 2.9)
6. Accounting matches vendor invoice in AP (Section 2.11)

**Path 2 — Subcontract (Outside Processing):**
1. Job routing includes outside processing operation (e.g., heat treat)
2. Upon job release or manual trigger: system generates subcontract PO linked to job and operation
3. PO sent to outside processing vendor
4. Parts ship to vendor with subcontract traveler
5. Upon return: Receiving posts outside processing receipt back to job
6. Vendor invoice matched in AP

**Path 3 — Blanket PO:**
1. Negotiate long-term agreement with vendor (e.g., annual steel contract)
2. Create blanket PO with total quantity/amount and contract period
3. Release individual orders against blanket as needed
4. Monitor remaining blanket quantity/amount

### Limitations

- **No automated PO approval routing:** Purchase request approval workflows are basic. Complex multi-level approval hierarchies require workarounds.
- **Blanket PO visibility:** Reporting on remaining blanket PO balances requires specific reports; there is no dashboard widget for this natively.
- **Subcontract PO timing:** Subcontract POs are not always generated at the most useful moment. Establish a procedure for when subcontract POs are created (at job release vs. when parts are ready to ship to vendor).

---

## 2.8 Material Control / Inventory

### What It Does

The Material Control module manages all physical inventory in the shop, including raw materials (steel bar, sheet, plate), standard purchased components, and finished goods. It tracks quantity on hand by bin location, values inventory using configurable cost methods, supports reorder point management, and provides an MRP-lite forecast utility.

### Key Screens and Features

| Screen / Feature | Description |
|-----------------|-------------|
| Material Master Record | Part number, description, unit of measure, cost method, reorder point, vendor, bin location(s) |
| Bin Location Tracking | Assign materials to specific physical locations; supports multiple bins per material |
| Cost Methods | FIFO, LIFO, Standard Cost, Average Cost — set per material record |
| Reorder Points | Minimum quantity trigger; Stale Inventory Report and MRP utility use this |
| Quick View Material Forecast Utility | MRP-lite tool; shows projected demand from open jobs vs. on-hand quantity |
| Physical Inventory | Freeze inventory counts; enter physical counts; post adjustments |
| Lot Control | Assign lot numbers to received material batches; trace lot through jobs |
| Heat Number Tracking | Track steel heat certificates by lot; supports AS9100 material traceability |
| Vendor Owned Inventory | Track consignment stock; material is in the shop but owned by vendor until consumed |
| Material Selling Price Update Utility | Bulk update selling prices for materials sold as products |
| Material Cost Update Utility | Bulk update standard costs for cost roll-up |
| Stale Inventory Report | Identify materials with no movement in a defined period |
| Transaction Register Report | Full audit trail of all inventory transactions (receipts, issues, adjustments) |

### Who Uses It

- **Purchasing / Inventory Control:** Manage material records, reorder, physical counts
- **Production:** Allocate and issue materials to jobs
- **Accounting:** Inventory valuation, cost of goods sold

### Dependencies and Prerequisites

Before using this module, you must:

1. Configure **Material Types** in Table Maintenance (e.g., Steel, Aluminum, Purchased Component)
2. Configure **Units of Measure** in Table Maintenance (LB, FT, EA, IN, etc.)
3. Configure **Bin Locations** in Table Maintenance — at minimum one default bin
4. Decide on **cost method** for each material type before entering any transactions. Changing cost methods after transactions exist is complex and may require ECI support.
5. Decide on **lot control** requirements before receiving any material. Enabling lot control after the fact requires retroactive lot assignment.

### Common Workflow Entry Points

**Material Master Setup:**
1. Table Maintenance > Materials > New
2. Enter part number, description, UOM, cost method
3. Assign default vendor, lead time, reorder point
4. Assign default bin location

**Physical Inventory Process:**
1. Inventory > Physical Inventory > Freeze Inventory (locks current counts)
2. Print count sheets by bin location
3. Conduct physical count
4. Enter counted quantities
5. Review variance report
6. Post adjustments — variance posts to G/L inventory adjustment account

**MRP / Forecast:**
1. Inventory > Quick View Material Forecast Utility
2. Select material or material type
3. Review: quantity on hand, quantity allocated to open jobs, quantity on order (open POs), projected shortage

### Limitations

- **Not a full MRP system:** The Quick View Material Forecast Utility is a planning aid, not a full MRP with explosion through multi-level BOMs. Complex assemblies with many component levels may require manual planning.
- **Physical inventory freeze:** During a physical inventory freeze, no inventory transactions can be posted. Plan physical counts during non-production periods.
- **Lot control retroactive enabling:** Cannot cleanly enable lot control after inventory has been received without lots. ECI professional services or database intervention may be required.
- **No native barcode label printing for inventory:** Bin labels and material labels must be formatted and printed separately or via a third-party label printing solution.

> **Common Mistake:** Setting cost method to Standard Cost but never running the Material Cost Update Utility to keep standard costs current. Stale standard costs cause inaccurate job costing variances.

---

## 2.9 Receiving

### What It Does

The Receiving module processes the physical receipt of materials and outside processing services against open purchase orders. Receipts update inventory quantities in real time, assign lot numbers (if lot control is enabled), and provide the AP matching trigger for vendor invoice payment.

### Key Screens and Features

| Screen / Feature | Description |
|-----------------|-------------|
| Receive Against PO | Select open PO; enter quantity received; confirm bin location |
| Partial Receipt | Receive partial quantity; PO remains open for balance |
| Bin Location Assignment | Assign received material to specific bin at time of receipt |
| Lot Number Assignment | If lot control is enabled, assign or confirm lot number at receipt |
| Outside Processing Receipt | Receive a subcontract operation back to the job; updates job routing status |
| Receipt Print | Print receiving report for documentation |
| Over-Receipt Warning | System warns if quantity received exceeds PO quantity |
| Packing Slip Matching | Record vendor packing slip number for AP matching |

### Who Uses It

- **Receiving Staff:** Process all incoming material and service receipts
- **Purchasing:** Monitor receipt status against open POs
- **Accounting:** Use receipt records for three-way match (PO / Receipt / Vendor Invoice) in AP

### Dependencies and Prerequisites

Before using this module, you must:

1. **Open Purchase Orders** must exist — Receiving can only post against open POs
2. **Bin Locations** must be configured in Table Maintenance
3. **Lot Control** must be enabled on material records if lot tracking is required at receipt
4. Receiving staff must have access permissions to the Receiving module (User Administration)

### Common Workflow Entry Points

1. Material arrives at dock with vendor packing slip
2. Receiving staff opens Receiving module
3. Search for PO by PO number, vendor, or material
4. Select PO; verify expected quantities and material
5. Enter quantity received; assign bin location
6. If lot control: enter or scan lot/heat number from material cert
7. Enter vendor packing slip number for AP reference
8. Post receipt — inventory updates immediately; PO status updates
9. File physical packing slip and material certifications (certs may also be attached in system)
10. Accounting uses receipt record for vendor invoice matching in AP

### Limitations

- **No receiving inspection integration in base module:** Receiving posts to inventory without a quality hold step unless Quality Control quarantine is actively configured. If incoming inspection is required, configure quarantine bins and establish a procedure.
- **Outside processing receipt:** Receiving a subcontract operation back to a job requires knowing the job number and operation number. Ensure subcontract travelers accompany parts to/from vendors.

---

## 2.10 Accounts Receivable (A/R)

### What It Does

The Accounts Receivable module manages all customer invoicing, payment collection, and AR aging. Invoices are generated based on posted packing lists (shipment-triggered) or job completion, depending on configuration. The module tracks outstanding balances, applies payments, and supports AR adjustments (credit memos, write-offs).

### Key Screens and Features

| Screen / Feature | Description |
|-----------------|-------------|
| Invoice Generation | Manual invoice creation or automatic from posted packing list |
| Invoice Detail | Customer, invoice date, due date, line items (from job/packing list), tax (manual), terms |
| Customer Payment Entry | Apply check, ACH, credit card, or other payments to open invoices |
| AR Aging Report | Aging buckets (current, 30, 60, 90, 120+ days); by customer or summary |
| Credit Memo | Create credit against a prior invoice; apply to future invoices or issue refund |
| AR Adjustment | Write-off bad debt; adjust invoice amounts |
| Statement Generation | Print or email customer statements of open balance |
| Cash Receipts Journal | Summary report of all payments received in a period |

### Who Uses It

- **Accounting / AR Staff:** Primary users for all invoicing and cash application
- **Sales:** Monitor customer payment status; flag overdue accounts

### Dependencies and Prerequisites

Before using this module, you must:

1. **Shipping / Packing Lists must be posted** (Section 2.6) to auto-generate invoices from shipments
2. **Customer records** must include payment terms, billing address, and tax exempt status
3. **Chart of Accounts** must be configured (Section 2.12) for AR posting accounts
4. If integrating with QuickBooks: understand what JobBoss posts to QuickBooks vs. what stays in JobBoss (Section 2.18)

### Common Workflow Entry Points

1. Shipping posts packing list (Section 2.6 Step 4)
2. AR staff opens Accounts Receivable > Invoices; pending invoice appears
3. Review invoice for accuracy; add any manual line items if needed
4. Note: **Sales tax is NOT automatically calculated** — must be entered manually or handled in QuickBooks
5. Post invoice; system assigns invoice number
6. Print or email invoice to customer
7. Upon customer payment: enter payment in Customer Payment Entry; apply to open invoice(s)
8. Month-end: run AR Aging Report; follow up on overdue accounts

### Limitations

- **Sales tax:** JobBOSS does not perform automated sales tax calculation or filing. Tax must be manually entered on invoices or managed in QuickBooks. This is a significant limitation for shops with sales tax obligations.
- **Credit card processing:** Native credit card processing is not built into the base AR module. Integration with a payment processor requires third-party add-on.
- **QuickBooks sync:** When using QuickBooks integration, revenue posting is by product code. Verify the product code mapping before go-live to ensure reconciliation works correctly. (See Section 2.18.)

> **Common Mistake:** Failing to review auto-generated invoices before posting. If the packing list had an error (wrong quantity, wrong job linked), the invoice will reflect that error. Always review before posting.

---

## 2.11 Accounts Payable (A/P)

### What It Does

The Accounts Payable module manages all vendor payment obligations. It receives vendor invoices, matches them to PO receipts, schedules payments, generates checks or EFT payments, and produces cash requirement reports for cash flow planning.

### Key Screens and Features

| Screen / Feature | Description |
|-----------------|-------------|
| Vendor Invoice Entry | Enter vendor invoice number, date, amount; link to PO/receipt for matching |
| Three-Way Match | Match vendor invoice to PO and receipt record for payment authorization |
| Payment Terms | Auto-calculates due date based on vendor terms (Net 30, 2/10 Net 30, etc.) |
| Check Run | Select invoices due for payment; generate check batch |
| Check Printing | Print checks on pre-printed check stock; supports MICR printing |
| EFT / ACH | Electronic payment generation (configuration required) |
| Cash Requirements Report | Projects upcoming payment obligations by date range |
| Vendor Payment History | Review all payments made to a specific vendor |
| AP Aging Report | Outstanding vendor invoices by aging bucket |
| 1099 Tracking | Track 1099-eligible payments for year-end reporting |

### Who Uses It

- **Accounting / AP Staff:** All vendor invoice entry and payment processing
- **Purchasing:** Confirm receipt status for AP matching
- **Management:** Review cash requirements for cash flow planning

### Dependencies and Prerequisites

Before using this module, you must:

1. **Vendor records** complete with payment terms, remittance address, 1099 status
2. **Receiving records** complete (Section 2.9) for three-way match
3. **Chart of Accounts** configured (Section 2.12) for AP liability accounts
4. Check stock pre-printed with company name and bank info (if printing checks)

### Common Workflow Entry Points

1. Vendor invoice arrives (mail, email, vendor portal)
2. AP staff enters invoice in Accounts Payable
3. Link invoice to PO number; system retrieves receipt records
4. Verify quantities and amounts match (three-way match)
5. If match confirmed: approve invoice for payment
6. At payment run: select due invoices; generate check or EFT batch
7. Print and mail checks (or transmit EFT file to bank)
8. System posts payment; updates vendor balance and G/L

### Limitations

- **No built-in invoice scanning / OCR:** Vendor invoices must be manually keyed. Integration with invoice automation (e.g., via Makini) is an option for high-volume AP.
- **1099 reporting:** JobBOSS tracks 1099 amounts but may not generate 1099 forms directly. Verify with ECI whether 1099 form printing is supported in the current version or if external processing is needed.

---

## 2.12 General Ledger (G/L)

### What It Does

The General Ledger module maintains the chart of accounts and records all financial transactions from operational modules (AR, AP, Inventory, Payroll) into period-based accounting records. It supports period-end close, financial statement generation, and QuickBooks integration.

### Key Screens and Features

| Screen / Feature | Description |
|-----------------|-------------|
| Chart of Accounts | Define account numbers, types (asset, liability, equity, revenue, expense), and descriptions |
| Journal Entries | Manual journal entries for adjustments and corrections |
| Period-End Close | Lock prior periods; prevent posting to closed periods |
| Financial Statements | Income Statement, Balance Sheet, Trial Balance — generated from G/L |
| Account Balances | View current period and year-to-date balances by account |
| Transaction Detail | Drill down from account balance to individual transactions |
| QuickBooks Export | Post G/L transactions to QuickBooks (see Section 2.18) |

### Who Uses It

- **Accounting / Controller:** All G/L management, period close, financial reporting
- **Management:** Financial statements review

### Dependencies and Prerequisites

Before using this module, you must:

1. Complete Chart of Accounts setup before any transactions are entered in any other module
2. Map G/L accounts to all posting categories (AR, AP, Inventory, COGS, etc.) in Table Maintenance
3. Establish fiscal year and period structure

> **Before using this module, you must** complete the Chart of Accounts and all G/L account mapping in Table Maintenance. Transactions posted without proper G/L mapping will create posting errors that are difficult to unwind.

### Limitations

- **Not a full accounting system for complex entities:** JobBOSS G/L is adequate for a single-entity small shop. Multi-entity, multi-currency, or complex consolidation requirements exceed its capabilities. Many shops use JobBOSS for operational costing and QuickBooks or another accounting system as the book of record.
- **QuickBooks as primary:** Many small shops configure JobBOSS G/L minimally and use QuickBooks as the true accounting system. In this configuration, understand exactly what syncs and what does not (Section 2.18).

---

## 2.13 Payroll

### What It Does

The Payroll module provides basic payroll processing for shops that want to manage payroll within JobBOSS. It captures labor hours from Shop Floor Data Collection, applies pay rates, calculates gross pay, and generates payroll records. For shops with complex payroll needs, a third-party integration is available.

### Key Screens and Features

| Screen / Feature | Description |
|-----------------|-------------|
| Payroll Processing | Calculate gross pay from hours collected via SFDC or manual entry |
| Employee Pay Rates | Regular, overtime, and shift differential rates per employee |
| Pay Period Configuration | Weekly, bi-weekly, semi-monthly — configured in Table Maintenance |
| Payroll Register | Summary report of pay by employee for the period |
| CenterPoint Payroll Integration | Third-party integration for full payroll processing (tax calculations, direct deposit, W-2, etc.) |

### Who Uses It

- **Accounting / HR:** Payroll processing and review

### Dependencies and Prerequisites

1. **Employee records** must be configured with pay rates, pay type (hourly/salary), and tax withholding information
2. **Shop Floor Data Collection** must be operational and accurate if using SFDC hours for payroll
3. For CenterPoint integration: CenterPoint Payroll license and integration configuration required

### Limitations

- **Base payroll module is limited:** JobBOSS's native payroll does not handle tax filing, direct deposit, W-2 generation, or benefits deductions at a production level. Most shops outsource payroll to ADP, Paychex, or use CenterPoint.
- **Recommendation for Die-Mension:** Use the payroll module only for labor cost tracking tied to jobs. Process actual payroll through an external payroll service. The hours data from SFDC is the valuable output from this module.

---

## 2.14 Quality Control

### What It Does

The Quality Control module provides basic inspection checkpoints within job routings, a quarantine inventory management process, and document management with revision control. It supports the operational aspects of ISO 9001 and AS9100 compliance at a foundational level.

### Key Screens and Features

| Screen / Feature | Description |
|-----------------|-------------|
| Inspection Operations | Add inspection steps to job routings with pass/fail criteria |
| Quality Control Quarantine Inventory Manager | Utility to manage quarantined material; move material to/from quarantine bin |
| Hold / Quarantine | Place specific job quantities on quality hold; prevent shipping until released |
| Document Management | Attach quality documents (procedures, work instructions) to jobs, parts, or customers |
| Revision Control | Track document revision levels; link to part revision history |
| NCR (Non-Conformance Report) | Basic NCR creation and tracking |

### Who Uses It

- **Quality Inspectors:** Record inspection results; manage quarantine
- **Quality Manager:** Manage documents, NCRs, corrective actions
- **Production:** Reference work instructions attached to jobs

### Dependencies and Prerequisites

1. Quality operations must be added to operation codes in Table Maintenance
2. Quarantine bin location must be configured in Inventory
3. Document management requires a file storage location accessible to JobBoss

### Limitations

- **Base QC module is not AS9100-compliant on its own:** The built-in Quality Control module provides the framework but lacks the audit trail, digital signature, and strict revision control required for AS9100 certification. The **uniPoint Quality add-on** (Section 2.15) is required for full AS9100 compliance.
- **NCR tracking:** Basic NCR creation is available but lacks full CAPA (Corrective and Preventive Action) workflow. uniPoint provides this.

---

## 2.15 uniPoint Quality (Add-On)

### What It Does

uniPoint is a separately licensed Quality Management System (QMS) that integrates with JobBOSS to provide a fully compliant quality environment for AS9100, ISO 9001, and similar standards. It provides a vaulted document control system, digital signatures, encrypted audit trails, non-conformance reporting, employee training records, and full CAPA workflow.

### Key Screens and Features

| Screen / Feature | Description |
|-----------------|-------------|
| Vaulted Document Environment | Documents stored in a controlled vault; access restricted by role |
| Strict Revision Control | Documents cannot be edited without creating a new revision; prior revisions archived |
| Digital Signatures | Electronic approval signatures with date/time stamps; meets 21 CFR Part 11-style requirements |
| Non-Revocable Approvals | Once approved, a document approval cannot be removed retroactively |
| Encrypted Passwords | User credentials for document approval are encrypted and auditable |
| NCR Generation | Create, track, and close non-conformance reports with root cause and corrective action |
| Employee Training Records | Link document revisions to required employee training; track training completion |
| Audit Trail Documentation | Every action in the QMS is logged with user, timestamp, and action type |
| CAPA Workflow | Structured Corrective and Preventive Action process with due dates and closure verification |
| Supplier Quality | Track supplier non-conformances; manage approved supplier list |

### Who Uses It

- **Quality Manager:** System administrator; approves documents; manages NCRs and CAPAs
- **All Employees:** View assigned documents; acknowledge training records
- **External Auditors:** Review audit trail and document vault during certification audits

### Dependencies and Prerequisites

1. **Separate license required** — uniPoint is purchased and licensed independently from JobBOSS
2. Integration configuration between uniPoint and JobBOSS must be completed by ECI or uniPoint professional services
3. Quality Manager must complete uniPoint administrator training before deploying to staff
4. Existing paper-based or informal quality documents must be digitized and loaded into the vault as part of implementation

### Limitations

- **Separate system:** uniPoint is not fully embedded in the JobBOSS UI. Users may need to switch between applications, depending on integration configuration.
- **Implementation complexity:** Migrating to a vaulted QMS requires significant upfront document preparation. Budget 40–80 hours of quality staff time for initial document loading and review.
- **Training requirement:** All employees who must acknowledge documents require basic uniPoint training. Plan this into the implementation timeline.

> **Note for Die-Mension:** If AS9100 certification is a business goal, uniPoint is the recommended path. Discuss with ECI during implementation planning to ensure uniPoint is included in the subscription and implementation scope.

---

## 2.16 Reporting Module

### What It Does

JobBOSS includes a built-in reporting infrastructure for operational, financial, and management reporting. Reports span all modules and are available as standard (pre-built) reports, user-defined reports, and custom reports created in Crystal Reports (on-premise) or Stimulsoft (cloud).

### Key Screens and Features

| Screen / Feature | Description |
|-----------------|-------------|
| Standard Reports Library | Pre-built reports for every module; accessible from within each module's menu |
| User Defined Reports Section | In-system report builder for creating custom list/detail reports without SQL knowledge |
| Crystal Reports (On-Premise) | Full-featured report development tool; `.rpt` files stored on server; requires Crystal Reports runtime |
| Stimulsoft (Cloud) | Embedded web-based report designer in JobBOSS²; supports PDF, Excel, Word export |
| Estimated vs. Actual Reports | Compare quoted cost to actual job cost by labor, material, and outside processing |
| Job Costing Reports | Detail and summary job cost reports by job, customer, date range |
| Inventory Reports | On-hand, transaction register, stale inventory, reorder, lot tracking |
| Scheduling Reports | Work center load, job schedule, past-due jobs |
| AR / AP Reports | Aging, cash receipts, cash requirements, vendor payment history |
| Financial Statements | Income statement, balance sheet, trial balance (from G/L) |

### Common Report Categories and Use Cases

| Category | Key Reports | Primary Users |
|----------|------------|--------------|
| Job Costing | Estimated vs. Actual, Job Cost Detail | Management, Estimating |
| Production | Open Jobs, Work in Process, Job Traveler | Production, Scheduling |
| Inventory | On Hand, Transaction Register, Stale Inventory | Purchasing, Inventory |
| Financial | AR Aging, AP Aging, Cash Requirements | Accounting |
| Scheduling | Work Center Load, Past Due Jobs | Scheduling, Management |
| Quality | NCR Log, Inspection Results (with uniPoint) | Quality |

### Limitations

- **Crystal Reports dependency (on-premise):** Standard reports require Crystal Reports runtime installed on every workstation that runs reports. Maintaining this runtime across workstation updates is an ongoing IT task.
- **User Defined Reports limitations:** The built-in user-defined report builder is useful for simple lists but cannot produce complex multi-table reports. Those require Crystal Reports or Stimulsoft.
- **Cloud report customization:** In JobBOSS², the Stimulsoft designer is available but modifying core standard reports may require ECI involvement. User-created reports in Stimulsoft are stored separately from standard reports.

---

## 2.17 Dashboards & KPIs

### What It Does

The Dashboards & KPIs module provides role-based visual dashboards showing real-time operational metrics. It consolidates data from production, scheduling, inventory, and financial modules into configurable widget-based displays for management decision-making.

### Key Screens and Features

| Screen / Feature | Description |
|-----------------|-------------|
| KPI Dashboard Configuration | Configure which widgets appear on each user's home screen |
| Role-Based Views | Different dashboard layouts for different roles (scheduler, manager, accounting) |
| Real-Time Job Status Widgets | Visual summary of open jobs by status (on time, at risk, past due) |
| Capacity Utilization Indicators | Work center load vs. capacity; highlights bottlenecks |
| Revenue and Billing Widgets | Invoiced amounts, outstanding AR, recent quotes |
| Inventory Alerts | Materials below reorder point; stale inventory flags |

### Who Uses It

- **Shop Managers:** Monitor production health at a glance
- **Schedulers:** Capacity and at-risk job summary
- **Accounting:** Revenue, billing, and AR summary

### Limitations

- **Dashboard configuration granularity:** Widget options are pre-defined by ECI. Shops cannot create entirely custom widgets without development work.
- **Tier dependency:** Advanced dashboard and KPI features are primarily a Platinum tier feature. Silver and Gold users may have limited dashboard options.

---

## 2.18 QuickBooks Integration

### What It Does

The QuickBooks integration module synchronizes financial data between JobBOSS and QuickBooks Desktop or QuickBooks Online. It allows shops to use JobBOSS for all operational management (quoting, jobs, purchasing, shipping) while maintaining QuickBooks as the financial book of record.

### Key Screens and Features

| Feature | What Syncs | Direction |
|---------|-----------|-----------|
| Customer Sync | Customer records | Bi-directional (setup dependent) |
| Vendor Sync | Vendor records | Bi-directional (setup dependent) |
| Revenue Posting | Invoice revenue posted by Product Code | JobBOSS → QuickBooks |
| AP Sync | Vendor bills | JobBOSS → QuickBooks |
| Payments | Customer payments (configuration-dependent) | Varies |

### Critical Limitations

> **These limitations are known system constraints that MUST be understood before go-live:**

| What Does NOT Sync | Impact |
|-------------------|--------|
| Sales Tax | Must be entered manually in QuickBooks; JobBOSS invoices show no tax |
| Payment Terms | Terms set in JobBOSS do not carry to QuickBooks invoices |
| Discount Terms (e.g., 2/10 Net 30) | Must be managed in QuickBooks manually |
| Inventory Valuation Detail | Summary postings only; detailed inventory sub-ledger stays in JobBOSS |

### Prerequisites

1. **Product Codes** must be configured in JobBOSS and mapped to QuickBooks income accounts. Revenue posting reconciliation depends entirely on correct product code mapping.
2. QuickBooks must be accessible from the JobBOSS integration service (on-premise: direct QuickBooks file access; cloud: QuickBooks Online API)
3. Customer and vendor records should be synchronized before beginning transaction sync to prevent duplicate records

### Common Workflow Entry Points

1. Complete setup: configure product codes, map to QuickBooks accounts, sync customers and vendors
2. JobBOSS invoices are posted in AR (Section 2.10)
3. Integration runs (scheduled or manual) to push invoices to QuickBooks
4. Accounting reconciles QuickBooks to JobBOSS AR aging monthly
5. Payments entered in QuickBooks flow back to JobBOSS (or entered in both, depending on configuration)

### Limitations

- **QuickBooks Online vs. Desktop:** API connectivity differs between QBO and QBD. Verify with ECI which version is currently supported and which limitations apply to each.
- **Sync errors:** Duplicate customers, unmatched product codes, and closed accounting periods in QuickBooks are the most common causes of sync errors. Establish a weekly reconciliation check.

---

## 2.19 KnowledgeSync

### What It Does

KnowledgeSync is an alerting and workflow automation engine that monitors the JobBOSS database for user-defined conditions and sends automated notifications via email or text message. It operates as a background service that periodically queries the database and triggers alerts when conditions are met.

### Key Features

| Feature | Description |
|---------|-------------|
| Condition Monitoring | Define SQL-based conditions (e.g., job past due date, PO overdue, inventory below reorder) |
| Email Alerts | Send formatted email notifications to defined recipients |
| SMS / Text Alerts | Send text message notifications (carrier gateway required) |
| Scheduled Queries | Run alerts on a defined schedule (hourly, daily, real-time) |
| Alert Templates | Pre-built alert templates for common manufacturing conditions |
| Escalation | Route alerts to different recipients based on severity or time elapsed |

### Common Alert Use Cases for Die-Mension

| Alert Condition | Recipient | Trigger |
|----------------|-----------|---------|
| Job past due date | Shop Manager, Customer Service | Job due date < today AND status = Open |
| PO overdue (not received) | Buyer | PO required date < today AND receipt quantity < order quantity |
| Material below reorder point | Buyer | On-hand quantity < reorder point |
| Quote aging (no response after X days) | Salesperson | Quote date > X days AND status = Open |
| Invoice overdue | AR Staff | Invoice due date < today AND balance > 0 |

### Dependencies and Prerequisites

1. **Separately licensed** — KnowledgeSync is not included in the base JobBOSS subscription
2. Requires a Windows service host (on-premise: on the JobBOSS server; cloud: ECI manages or separate server)
3. SMTP email server credentials required for email alerts
4. SMS alerts require a carrier gateway account (e.g., via SMTP-to-SMS or Twilio)
5. Alert conditions are written in SQL — either use pre-built templates or engage ECI/KnowledgeSync support for custom conditions

### Limitations

- **SQL knowledge required for custom alerts:** Pre-built templates cover common scenarios. Custom alert conditions require SQL query writing against the JobBOSS database schema.
- **On-premise dependency:** For cloud deployments, KnowledgeSync must be separately hosted or managed by ECI. Confirm hosting arrangement with ECI at time of purchase.

---

## 2.20 CAD BOM Import

### What It Does

The CAD BOM Import feature allows engineering or estimating staff to import Bills of Materials directly from CAD systems or structured files, eliminating manual re-entry of part lists from engineering drawings into JobBOSS estimates and jobs.

### Supported Import Sources

| Source | Format | Notes |
|--------|--------|-------|
| SolidWorks | XML, Excel export | BOM exported from SolidWorks assembly |
| Autodesk Inventor | XML, Excel export | BOM exported from Inventor assembly |
| Excel | .xlsx, .xls | Structured BOM spreadsheet with defined column mapping |
| General CSV | .csv | Comma-separated BOM with header row |

### Key Features

| Feature | Description |
|---------|-------------|
| Column Mapping | Map import file columns to JobBOSS fields (part number, description, quantity, UOM) |
| Material Matching | Match imported parts to existing material records by part number |
| New Material Creation | Create new material records for unmatched parts during import |
| BOM Preview | Review imported BOM before committing to estimate or job |
| AI BOM Builder | Upload PDF drawings or images; AI extracts BOM data (see also Section 2.1) |

### Limitations

- **AI accuracy on complex drawings:** AI-based PDF/image parsing works best on clean digital PDFs. Scanned or hand-marked drawings produce less accurate results. Always review before using.
- **No direct CAD link:** This is an import function, not a live CAD integration. Changes to the CAD model after import require a new import or manual update in JobBOSS.

---

## 2.21 CADLink

### What It Does

CADLink, developed by QBuild Corporation, is a third-party integration product that provides deeper connectivity between CAD systems and JobBOSS beyond the native BOM import. It is a separately purchased and licensed product.

### Key Features

- Enhanced BOM extraction from supported CAD systems
- Part attribute mapping to JobBOSS fields
- Automated creation of material and operation records from CAD data
- Supports additional CAD formats beyond native JobBOSS import

### Dependencies and Prerequisites

1. **Separate license required** from QBuild Corporation
2. CADLink installation and configuration must be coordinated between QBuild, ECI, and the shop's IT staff
3. Compatible CAD system must be installed and accessible

> **Note for Die-Mension:** Evaluate the AI BOM Builder (Section 2.20) before purchasing CADLink. For many small shops, the native BOM import and AI parsing are sufficient. CADLink provides value when BOM import volume is high or when CAD-to-JobBOSS workflow automation is a priority.

---

## 2.22 Makini Integration

### What It Does

Makini.io is an API integration platform that provides a standardized set of 30+ manufacturing data entities for connecting JobBOSS to external software systems. It abstracts the complexity of direct SQL or proprietary API integration into a normalized REST API layer.

### Supported Entity Types (Examples)

| Entity Category | Examples |
|----------------|---------|
| Production | Jobs, Operations, Work Centers, Routings |
| Procurement | Purchase Orders, Vendors, Receipts |
| Inventory | Materials, Bin Locations, Transactions |
| Customer | Customers, Sales Orders, Quotes |
| Financial | Invoices, Payments |
| Quality | NCRs, Inspections |

### Common Integration Use Cases

- Connect JobBOSS to a customer portal or ERP
- Push job status updates to a business intelligence (BI) tool
- Sync JobBOSS customer and order data to a CRM (e.g., Salesforce, HubSpot)
- Feed production data to a reporting or analytics platform

### Dependencies and Prerequisites

1. **Separate license required** from Makini.io
2. Requires API key provisioning by ECI and Makini
3. Integration development requires REST API / JSON knowledge on the consuming system side

### Limitations

- **Cloud only (primarily):** Makini integration is best suited for the JobBOSS² cloud deployment. On-premise direct SQL access may be simpler for on-premise shops with IT resources.
- **Entity coverage:** Not all JobBOSS data entities are exposed via Makini. Verify that the specific data needed for an integration is within the 30+ supported entities before committing to this approach.

---

## 2.23 Alora Machine Intelligence

### What It Does

Alora Machine Intelligence is a third-party machine monitoring platform that integrates with JobBOSS to provide real-time machine utilization data, Overall Equipment Effectiveness (OEE) metrics, and production analytics. It connects physical machines to the software layer.

### Key Features

| Feature | Description |
|---------|-------------|
| Machine Monitoring | Real-time monitoring of machine state (running, idle, down) via hardware connectors |
| OEE Dashboard | Availability, Performance, and Quality metrics per machine |
| Utilization Reporting | Machine utilization percentage by shift, day, week |
| Downtime Tracking | Reason codes for machine downtime; trend analysis |
| JobBOSS Integration | Link machine utilization data to specific jobs running on each machine |

### Dependencies and Prerequisites

1. **Separate license required** from Alora Machine Intelligence
2. Hardware installation on each monitored machine (MTConnect, OPC-UA, or Alora-proprietary connectors)
3. Network connectivity from each machine to Alora's cloud or on-premise server
4. Integration configuration between Alora and JobBOSS (typically via Makini or direct API)

### Limitations

- **Hardware cost:** Machine monitoring requires physical hardware per machine. Total cost scales with machine count.
- **Implementation time:** Installing and configuring hardware on all machines in a tool and die shop requires planned downtime and IT/engineering coordination.

---

## 2.24 Table Maintenance

### What It Does

Table Maintenance is the master data configuration hub for JobBOSS. Nearly every other module depends on data configured in Table Maintenance. It is the first area that must be fully configured before the system can be used for production purposes.

### Key Configuration Areas

| Table | What It Configures | Used By |
|-------|--------------------|---------|
| Work Centers | Physical or logical production areas; available hours, efficiency, labor/burden rates, calendar | Scheduling, Job Management, Costing |
| Employees | Employee records; pay rates, skills, badge numbers, hire date | SFDC, Payroll, Job Management |
| Operations | Standard operation codes (Turning, Milling, Grinding, etc.) with default times and rates | Estimating, Job Management |
| Bin Locations | Physical storage locations in the shop/warehouse | Inventory, Receiving |
| Collection Terminals | SFDC terminal records; one per physical terminal or device | Shop Floor Data Collection |
| Customer Classes | Group customers for reporting and pricing tiers | Order Entry, AR |
| Vendor Classes | Group vendors by type (material, service, outside processing) | Purchasing, AP |
| Material Types | Categorize materials (steel, aluminum, carbide, hardware) | Inventory, Purchasing |
| Units of Measure | EA, LB, FT, IN, SQ FT, etc. | All modules |
| Chart of Accounts | G/L account codes and types | G/L, AR, AP |
| Markup Codes | Markup percentages by category for estimating | Estimating |
| Shipping Carriers | FedEx, UPS, LTL carriers | Shipping |
| Pay Periods | Weekly, bi-weekly, semi-monthly | Payroll |
| Reason Codes | Downtime, scrap, NCR reason codes | Quality, Scheduling |
| Tax Codes | Sales tax rates and jurisdictions | AR, Order Entry |
| Product Codes | Revenue categories for QuickBooks mapping | AR, QuickBooks Integration |

### Configuration Sequence (Recommended Order)

The following sequence prevents dependency errors during initial setup:

1. Units of Measure
2. Material Types
3. Chart of Accounts
4. Vendor Classes
5. Customer Classes
6. Tax Codes / Product Codes
7. Operations (with rates)
8. Work Centers (with calendars and rates)
9. Employees (with pay rates and badge numbers)
10. Bin Locations
11. Collection Terminals (if using SFDC)
12. Markup Codes
13. Shipping Carriers

> **Before using ANY other module, you must** complete Table Maintenance configuration through at minimum steps 1–9 above. Attempting to create estimates, jobs, or purchase orders without this foundation will produce incomplete records and system errors.

> **Common Mistake:** Configuring work centers without setting up the work center calendar (shift hours, holidays, available days). A work center without a calendar has zero available capacity, causing scheduling to show 0% capacity or error.

---

## 2.25 User Administration

### What It Does

User Administration manages user accounts, password policies, permission levels, and role assignments across the JobBOSS system. It controls which users can access which modules and perform which actions.

### Key Features

| Feature | Description |
|---------|-------------|
| User Account Creation | Create login credentials for each system user |
| Role Assignment | Assign users to pre-defined roles (Admin, Estimator, Scheduler, Shop Floor, Accounting, etc.) |
| Module Access Control | Enable or disable module access per user or role |
| Permission Levels | Read-only vs. read/write vs. admin per module |
| Password Policy | Enforce minimum password length, complexity, and expiration |
| Audit Logging | Track user logins and key actions (varies by module) |

### Known Limitations (As Documented in Ideas Portal)

The permission system in JobBOSS has documented granularity gaps that are known to the ECI development team. The following Ideas Portal submissions are relevant:

| Ideas Portal ID | Description |
|----------------|-------------|
| JBCORE-I-1023 | Request for more granular permissions within the Estimating module |
| JBCORE-I-1068 | Request for field-level permission control (hide specific fields from certain users) |
| JBCORE-I-1100 | Request for read-only access to Job Management without edit capability on all sub-tabs |

> **Note for Die-Mension:** Vote on these Ideas Portal items at customerportal.ecisolutions.com to signal priority to ECI. In the interim, work with ECI support to find the best available permission configuration for your user roles. If an employee should not have access to cost data on jobs, verify this is achievable before assigning them a role that includes Job Management.

### Common Workflow Entry Points

1. System Admin > User Administration > New User
2. Enter username, email, temporary password
3. Assign role
4. Configure module access for any role-level overrides
5. User logs in; prompted to change password on first login
6. Periodic review: deactivate accounts for terminated employees immediately

> **Common Mistake:** Not deactivating user accounts when employees leave the company. JobBOSS does not auto-deactivate accounts based on employee termination in the HR/Employee record. Deactivation must be done manually in User Administration.

---

## 2.26 System Administration

### What It Does

The System Administration module provides tools for database management, backup configuration, upgrade management, SQL Server maintenance, and ODBC connection management. It is primarily used by the shop's IT administrator or ECI's managed services team.

### Key Features

| Feature | Description |
|---------|-------------|
| Database Backup Configuration | Schedule automated SQL Server backups; define backup path and retention |
| SQL Server Maintenance | Index rebuilding, statistics updates, log file management |
| Upgrade Management | Download and apply software updates (on-premise); monitor ECI release notes |
| ODBC Connection Management | Configure ODBC data source for Crystal Reports and third-party connections |
| Error Log Review | Review system and application error logs for troubleshooting |
| Database Connection Settings | Configure connection strings for the application to reach SQL Server |
| User Session Management | View and terminate active user sessions (on-premise) |

### Deployment-Specific Notes

| Task | Cloud (JobBOSS²) | On-Premise (JobBoss) |
|------|-----------------|---------------------|
| Database Backups | ECI manages; shop has no direct control | Shop IT or DBA manages; critical responsibility |
| SQL Server Maintenance | ECI manages | Shop IT manages |
| Upgrades | ECI pushes automatically | Shop controls timing; must test before production |
| ODBC Access | Not available to shop | Full access; required for Crystal Reports |
| Disaster Recovery | ECI SLA-driven | Shop responsible; define and test RPO/RTO |

### On-Premise Backup Recommendations

For Die-Mension on-premise installations:

1. **Full database backup:** Nightly, to a location off the production server (NAS, cloud storage, or tape)
2. **Transaction log backups:** Every 1–4 hours during production hours for point-in-time recovery
3. **Backup testing:** Monthly restore test to verify backup integrity
4. **Retention:** Keep at minimum 30 days of daily backups; keep monthly backups for 1 year
5. **Database name:** Confirm whether the database is named `JobBoss32`, `production`, or a custom name — back up this specific database

> **Common Mistake:** Backing up only the application files and ignoring the SQL Server database. The application files without the database are useless. The SQL Server database is the only backup that matters for data recovery.

> **Before performing any upgrade** on the on-premise system, verify:
> 1. A full backup has been taken and tested
> 2. The release notes have been reviewed for breaking changes
> 3. The upgrade has been tested in a non-production environment if one is available
> 4. ECI support has been notified or a support case is open in case issues arise during upgrade

---

## Module Dependency Summary

The following table provides a quick reference for module dependencies. Use this when planning implementation sequence or troubleshooting missing functionality.

| Module | Must Configure First |
|--------|---------------------|
| Estimating & Quoting | Work Centers, Operations, Customers, Markup Codes |
| Order Entry | Customers, Estimating (if converting quotes) |
| Job Management | Work Centers, Operations, Employees, Order Entry or standalone |
| Scheduling | Work Centers (with calendars), Job Management |
| Shop Floor Data Collection | Employees (with badges), Collection Terminals, Job Management (numeric job numbers) |
| Shipping | Job Management, Customer shipping addresses |
| Purchasing | Vendors, Materials, Operations (for subcontracts) |
| Material Control | Material Types, Units of Measure, Bin Locations, Cost Method decision |
| Receiving | Purchase Orders, Bin Locations, Lot Control decision |
| Accounts Receivable | Customers, Chart of Accounts, Shipping (for shipment-triggered invoices) |
| Accounts Payable | Vendors, Chart of Accounts, Receiving |
| General Ledger | Chart of Accounts (configure before ALL other modules) |
| Payroll | Employees, SFDC (for hours) |
| Quality Control | Operations (for inspection operations), Bin Locations (for quarantine) |
| uniPoint Quality | Quality Control, separate uniPoint license |
| QuickBooks Integration | Chart of Accounts, Product Codes, QuickBooks account structure |
| KnowledgeSync | SQL Server access, SMTP configuration, separate license |
| Table Maintenance | Must be configured FIRST — foundation for all modules |
| User Administration | Table Maintenance complete |
| System Administration | IT infrastructure in place; SQL Server running |

---

*End of Section 1 and Section 2. These sections form the foundation reference for all subsequent sections of the Die-Mension JobBOSS Master Reference Manual.*

---

Here is a summary of what was written:

**Section 1: Product Overview & History** covers five subsections across approximately 350 lines:

- **1.1** traces the naming and ownership history from Shoptech/Exact Software through ECI's acquisitions, with a timeline table and practical notes for disambiguating legacy documentation.
- **1.2** details the three subscription tiers (Silver, Gold, Platinum) in a comparison table, both deployment models (cloud vs. on-premise), and approximate pricing.
- **1.3** provides full contact information for ECI, support channels, portal URLs, and a table of ECI's other products to prevent confusion.
- **1.4** documents the full technology stack including SQL Server database naming, reporting platforms (Crystal Reports vs. Stimulsoft), iOS app IDs, and all integration middleware (KnowledgeSync, Makini, CADLink, Alora).
- **1.5** provides an ideal customer profile matrix and explicit notes for Die-Mension as a precision tool and die shop.

**Section 2: Complete Module List and Overview** covers all 26 modules across approximately 1,100+ lines. Each module entry includes:

- A plain-language explanation of what it does
- Tables of key screens and features
- Who uses it
- A numbered prerequisites list with explicit "Before using this module, you must..." callouts
- Step-by-step workflow entry points (often multiple paths)
- A limitations section with "Common Mistake:" callouts where applicable

The section closes with a **Module Dependency Summary table** mapping every module to its required prerequisites, which provides an AI assistant or administrator a quick reference for sequencing configuration work.


---

No relevant skill applies to this documentation writing task. Proceeding with the full manual section.

```markdown
# SECTION 3: MATERIAL CONTROL DEEP DIVE

**Die-Mension Corporation — JobBOSS² Master Reference Manual**
**Brunswick, OH | Precision Tool and Die**
**Module Status: UNCONFIGURED — Requires Full Setup from Scratch**

---

> **IMPORTANT NOTICE FOR ALL USERS AND AI ASSISTANTS**
> This section documents the Material Control / Inventory module for Die-Mension, a small precision tool and die shop. As of the writing of this manual, the Material Control module is completely unconfigured. No bin locations have been created, no costing methods have been selected, no lot control has been enabled, and no physical inventory baseline has been established. Every step in this section must be completed in the order presented before any live inventory transactions occur. Skipping steps or performing them out of order will create data integrity problems that are extremely difficult or impossible to correct retroactively.

---

## 3.1 Overview and Why Material Control Matters

### What Material Control Does for a Tool and Die Shop

Material Control in JobBOSS/JobBOSS² is the module responsible for managing physical inventory — what you have on hand, where it is located, what it costs, and how it flows in and out of the shop. For Die-Mension, this includes all tool steels, carbide blanks, cutting tools, hardware, coolants, grinding wheels, and any other stocked items used in the production of dies, molds, and precision tooling.

A properly configured Material Control module gives Die-Mension the ability to:

- Know exactly how many pieces, pounds, or linear feet of any material are on hand at any moment
- Know the current cost basis of every item in inventory for accurate job quoting and financial reporting
- Trace every piece of tool steel from the purchase order, through the receiving dock, to a specific job — including which mill certification (heat number) covers that material
- Generate automatic purchasing alerts when materials drop below reorder points
- Prevent duplicate purchasing of materials already on order
- Allocate and consume materials against specific jobs so that job costs are accurate
- Support AS9100 / ISO 9001 compliance requirements through lot and heat number traceability
- Provide accurate financial statements by correctly valuing the raw material asset on the balance sheet

### The Consequences of NOT Configuring Material Control

Many small shops operate JobBOSS for years using only the Job Management and Purchasing modules, treating inventory as an afterthought. This approach creates compounding problems:

**Stockouts and Production Delays**
Without reorder points and on-hand tracking, the shop discovers material shortages only when a job is ready to start and the material is not on the shelf. Emergency purchases of tool steel at spot prices are significantly more expensive than planned purchases, and expedited shipping adds further cost. A single stockout on a critical job can cause a missed delivery date.

**Inaccurate Job Costs and Quoting Errors**
If material costs are entered manually on each job rather than pulled from a properly maintained inventory record, quoting errors accumulate. A shop quoting D2 tool steel at last year's price when the actual cost has increased 25% will systematically underprice work and erode margins invisibly. The cost problem only becomes visible at year-end when the books are closed.

**No Material Traceability**
Many of Die-Mension's customers — particularly those in automotive, aerospace, or defense supply chains — require documentation proving that the material used in their tooling came from a certified mill and meets specific chemistry requirements. Without lot/heat number control configured in JobBOSS², this traceability either does not exist or exists only as paper records that cannot be searched, audited, or cross-referenced to specific jobs. This is a significant compliance risk.

**Duplicate and Uncontrolled Purchasing**
Without an on-order quantity visible to the person placing purchase orders, it is common for a shop to order material that is already on an open PO. The material arrives, there is no place to properly receive it, and the on-hand count remains inaccurate.

**Inability to Support MRP / Material Planning**
The Quick View Material Forecast utility in JobBOSS² — the closest thing to a Material Requirements Planning (MRP) report in the system — requires accurate on-hand quantities, reorder points, and open job material requirements to function. Without these inputs, the report produces meaningless output.

### The Key Decision That Must Be Made BEFORE Any Transactions: Costing Method Selection

Before a single receiving transaction is entered, the costing method for each part must be selected. This is not a decision that can be deferred. Once receiving transactions exist in the system for a part, changing the costing method for that part is extremely difficult and in most configurations impossible without purging historical transaction data.

The costing method determines how JobBOSS² calculates the cost of material when it is consumed (issued to a job). It affects job costs, work-in-process valuation, cost of goods sold, and the balance sheet value of inventory.

**This decision requires input from the company's accountant or controller. Karen should not select a costing method without consulting with Die-Mension's accounting professional.**

See Section 3.3 for a full comparison of all available costing methods.

---

## 3.2 Prerequisites Checklist (Must Complete Before First Transaction)

The following items must be completed in the order listed. This is a hard dependency chain — items lower on the list depend on items higher on the list. Do not proceed to item N+1 until item N is complete.

> **AI ASSISTANT GUIDANCE:** If Karen asks you to help set up Material Control and any of the items below are not yet complete, stop and direct her to complete the prerequisite first. Do not attempt to work around missing prerequisites.

### Complete Prerequisites in This Order:

**Step 1 — Work Centers Must Exist in Table Maintenance**

Work centers define the physical production areas in the shop (e.g., EDM, Surface Grinder, Milling, Lathe). They are required before employees can be assigned to operations, and some material transactions are linked to work centers for cost tracking purposes.

- Navigate to: **Table Maintenance → Work Centers**
- Verify that all production work centers for Die-Mension are listed
- If any are missing, add them before proceeding
- Required fields: Work Center ID, Description, Department
- See Section 2 of this manual for full Work Center setup instructions

**Step 2 — Employees Must Be Set Up**

Employee records are required for labor tracking and for assigning responsibility to transactions. Receiving transactions, in particular, may record which employee performed the receipt.

- Navigate to: **Table Maintenance → Employees**
- Verify all shop employees and office staff are listed
- Required fields: Employee ID, Name, Department, Pay Rate (if tracking labor costs)
- See Section 2 of this manual for full Employee setup instructions

**Step 3 — Part Master Records Must Exist for All Stocked Materials**

Every material that will be tracked in inventory must have a Part Master record. The Part Master is the central record for a stockable item — it holds the part number, description, unit of measure, costing method, lot control setting, reorder point, and bin location assignments.

- Navigate to: **Inventory → Parts**
- For each material Die-Mension stocks (all tool steels, carbide, cutting tools, hardware, consumables), a Part record must exist
- If importing from a spreadsheet, use the import utility — see Section 3.5 for required fields
- Part numbers must be finalized before this step — changing part numbers after transactions exist is destructive

**Step 4 — Bin Locations Must Be Created in Table Maintenance**

Bin locations define the physical storage locations in the shop. JobBOSS² will not allow assignment of a bin location to a part record if the bin location does not already exist in Table Maintenance.

- Navigate to: **Table Maintenance → Bin Locations**
- Create all bin locations before assigning them to parts
- See Section 3.4 for full bin location setup instructions and naming conventions

**Step 5 — Bin Locations Must Be Assigned to Part Records**

After bin locations exist, return to each Part Master record and assign the appropriate bin location(s).

- Navigate to: **Inventory → Parts → [Part Record] → Bin Location tab**
- Assign at least one bin location to each stocked part
- Designate a default bin location for each part
- A part can have multiple bin locations (e.g., primary rack location plus an overflow location)

**Step 6 — Costing Method Must Be Selected on Each Part Record**

This is the most critical prerequisite. Before the costing method can be selected intelligently, Die-Mension's accountant must be consulted. See Section 3.3 for the full comparison.

- Navigate to: **Inventory → Parts → [Part Record] → General tab**
- Select the costing method field
- Apply consistently across similar material categories
- Document the selected method in this manual for future reference
- **ONCE SELECTED AND TRANSACTIONS EXIST, THIS CANNOT BE CHANGED WITHOUT SIGNIFICANT DATA RISK**

**Step 7 — Lot Control Must Be Enabled on Any Parts Requiring Traceability BEFORE First Receipt**

Lot control, once enabled on a part record, causes the system to prompt for a lot/heat number at every receipt and every issue. If lot control is not enabled before the first receipt of a part, the early receipts will have no lot assignment, creating a traceability gap.

- Navigate to: **Inventory → Parts → [Part Record] → General tab**
- Check the Lot Control checkbox for all tool steels and any other materials requiring heat number traceability
- This must be done before the first receipt of each part
- See Section 3.6 for full lot control configuration instructions

**Step 8 — Reorder Points Must Be Set**

Reorder points tell the Quick View Material Forecast utility when to alert for purchasing. Without reorder points, the MRP screen is blind to projected stockouts.

- Navigate to: **Inventory → Parts → [Part Record] → General tab**
- Enter the reorder point quantity for each part
- Use the formula in Section 3.7 to calculate appropriate reorder points
- Set at a minimum for all A-category and B-category materials

**Step 9 — Vendors Must Be Set Up with Terms and Lead Times**

Vendor records must exist before purchase orders can be generated. Lead times on vendor records feed into reorder point calculations and the MRP forecast.

- Navigate to: **Table Maintenance → Vendors**
- Verify all material suppliers are listed with current contact information
- Enter payment terms, lead times, and preferred ship methods
- Link vendors to part records as primary or secondary vendors
- See Section 4 of this manual for full Vendor setup instructions

> **CHECKPOINT:** Do not proceed with any live inventory transactions until all nine prerequisite steps above are marked complete. Print this checklist and have Karen sign and date each item as it is completed. Keep the signed checklist in the JobBOSS² configuration binder.

---

## 3.3 Costing Method Comparison Table

The costing method determines how the cost of inventory is calculated when material is consumed or sold. JobBOSS² supports four costing methods. The choice affects both operational job costing and financial reporting.

> **CRITICAL WARNING — READ BEFORE PROCEEDING**
>
> The costing method selected for each part is effectively permanent once receiving transactions exist. Changing the costing method after receipts have been entered requires purging transaction history for that part, which destroys audit trail data and creates accounting adjustments. In some configurations, a full system-level change to the costing method requires involvement from the JobBOSS² support team and can affect all historical financial reports. **This decision must be made with input from Die-Mension's accountant or CPA before going live. Karen must not select a costing method independently.**

### Costing Method Comparison

| Method | Description | Best For | Pros | Cons | Can Change After Transactions? |
|---|---|---|---|---|---|
| **FIFO** (First In, First Out) | The cost of the oldest inventory layer is used first when material is consumed. As older inventory is used up, the cost basis moves to newer, more expensive (or less expensive) receipts. | Manufacturing environments with stable material costs and where physical flow actually matches FIFO. Retail and food manufacturing. | Matches physical flow of most materials. Produces accurate current-value inventory on balance sheet. Accepted under US GAAP and IFRS. | In an inflationary environment, FIFO produces lower COGS and higher taxable income. Can create wide swings in job cost when old low-cost layers are replaced by new high-cost receipts. | Extremely difficult. Requires transaction purge per part. |
| **LIFO** (Last In, First Out) | The cost of the most recently received inventory is used first when material is consumed. Older lower-cost (or higher-cost) layers remain on the balance sheet. | US-based companies seeking to reduce taxable income during inflationary periods. | In inflation, LIFO produces higher COGS and lower taxable income. | Not permitted under IFRS. Creates increasingly understated inventory value on the balance sheet over time. Complexity of managing LIFO layers is high. If a material category is liquidated, a large taxable event can occur. Not recommended for small shops. | Extremely difficult. |
| **Standard Cost** | A predetermined cost is established for each part (the "standard"). All receipts and issues use this fixed cost regardless of actual purchase price. Variances between actual and standard are tracked separately. | Larger manufacturers with stable production processes and dedicated cost accounting staff. Automotive suppliers. Shops with very consistent material grades and sizes. | Job costs are highly predictable. Budget variance analysis is meaningful. Simplifies job costing calculations. | Requires ongoing maintenance of standard costs as market prices change. Standards must be updated at least annually — if not updated, all cost reports drift from reality. Significant accounting work to manage purchase price variance accounts. Requires dedicated accounting attention that Die-Mension may not have. | Extremely difficult. |
| **Average Cost** (Weighted Average) | The cost basis for a part is recalculated as a running weighted average every time new inventory is received. Each receipt blends the new cost with the existing average. When material is issued, the current weighted average cost is used. | Small to mid-size manufacturers with fluctuating material prices. Tool and die shops. Job shops. Any shop without a dedicated cost accountant maintaining standards. | Smooths out price fluctuations across receipts — no single expensive receipt spikes job costs dramatically. No standards to maintain. Simple to understand and audit. Resistant to manipulation. Works well for steel where prices move regularly. | Does not produce the tax advantage of LIFO in inflationary periods. Does not enable the variance analysis of standard costing. Average can be "diluted" if large quantities of old low-cost inventory are on hand when new expensive material arrives. | Extremely difficult. |

### Recommendation for Die-Mension

**Average Cost is the recommended costing method for Die-Mension across all material categories.**

Rationale:
- Tool steel prices (D2, H13, O1, A2, S7, M2) fluctuate with domestic steel markets and vary by supplier. A single coil or bar order at a different price than the last order should not cause a sharp spike in job cost — the average smooths this out.
- Die-Mension does not have a dedicated cost accountant to maintain standard costs. Without active maintenance, standard costs drift from reality and produce misleading job cost reports.
- LIFO creates accounting complexity and potential tax exposure on layer liquidation that is not appropriate for a shop of Die-Mension's size.
- FIFO can create job cost volatility when material layers turn over.
- Average Cost requires no ongoing maintenance beyond normal receiving transactions. The average updates automatically.

> **ACTION REQUIRED:** Before proceeding, Karen must schedule a meeting with Die-Mension's accountant to confirm the costing method selection. The accountant must sign off on this decision. Document the confirmed method here:
>
> Confirmed Costing Method: ___________________________
>
> Accountant Name: ___________________________
>
> Date Approved: ___________________________

---

## 3.4 Bin Location Setup (Step-by-Step)

### Why Bin Locations Matter

Bin locations in JobBOSS² are the virtual representation of physical storage locations in the shop. When material is received, it is received into a bin. When material is issued to a job, it is pulled from a bin. When a physical inventory count is performed, it is performed bin by bin.

Without bin locations:
- The system cannot track where material is physically located
- Multiple people searching for the same material waste time
- Physical inventory counts are conducted shop-wide in a single undifferentiated sweep rather than systematically by location
- Receiving clerks have no guidance on where to put incoming material

For Die-Mension, a precision tool and die shop, the most important bin locations are those that organize tool steel by form (bar, plate, rounds) and by grade. This allows the toolmakers to quickly locate the exact size and grade needed for a job without searching the entire shop.

### How to Create Bin Locations in Table Maintenance

**Navigation:** Main Menu → Table Maintenance → Bin Locations

**Step-by-Step Instructions:**

1. From the main JobBOSS² menu, click **Table Maintenance** in the left navigation panel or top menu.
2. In the Table Maintenance list, locate and click **Bin Locations**.
3. The Bin Location list screen opens, showing all existing bin locations (currently empty for Die-Mension).
4. Click the **New** button (or press **F4** if using keyboard shortcuts) to create a new bin location record.
5. In the **Bin Location ID** field, enter the bin location code. This code appears on pick lists, receiving documents, and count sheets. It must be unique. Maximum length varies by version but treat it as 10 characters for display purposes.
6. In the **Description** field, enter a human-readable description of the physical location (e.g., "Steel Rack Section A, Shelf 1").
7. Click **Save** (or press **F10**).
8. Repeat for each bin location.

### Naming Convention Recommendations for Die-Mension

Consistency in naming conventions is critical. Once transactions exist with a bin location name, renaming the bin is disruptive. Establish the naming convention before creating any bins and document it here.

**Recommended Naming Convention:** `[AREA]-[SECTION][NUMBER]`

| Bin Location ID | Description | Physical Location |
|---|---|---|
| `RACK-A1` | Steel Rack A, Position 1 | Main steel storage rack, left side, bottom shelf |
| `RACK-A2` | Steel Rack A, Position 2 | Main steel storage rack, left side, second shelf |
| `RACK-A3` | Steel Rack A, Position 3 | Main steel storage rack, left side, third shelf |
| `RACK-B1` | Steel Rack B, Position 1 | Main steel storage rack, right side, bottom shelf |
| `RACK-B2` | Steel Rack B, Position 2 | Main steel storage rack, right side, second shelf |
| `RACK-B3` | Steel Rack B, Position 3 | Main steel storage rack, right side, third shelf |
| `SHELF-C1` | Shelf C, Position 1 | Small parts/hardware shelving, left bay |
| `SHELF-C2` | Shelf C, Position 2 | Small parts/hardware shelving, center bay |
| `SHELF-C3` | Shelf C, Position 3 | Small parts/hardware shelving, right bay |
| `FLOOR-STOCK` | Floor Stock — General | Material staged on shop floor, not racked |
| `COIL-STORAGE` | Coil and Flat Plate Storage | Flat plate and oversized stock area |
| `DROPS` | Remnant/Drop Bin | Cut-off pieces and usable remnants |
| `QUARANTINE` | Quarantine Hold Area | Material awaiting inspection or disposition |
| `CARBIDE-SHELF` | Carbide Blanks Shelf | Dedicated carbide blank storage |
| `TOOL-CRIB` | Tool Crib — Cutting Tools | Drill bits, end mills, inserts, taps |
| `COOLANT-RACK` | Coolant and Lubricant Storage | Cutting fluids, coolant, way oil |
| `HARDWARE-BIN` | Hardware Bins | Screws, dowel pins, springs, fasteners |
| `WHEEL-SHELF` | Grinding Wheel Storage | Grinding and dressing wheels |
| `RECEIVING` | Receiving Dock — In Process | Material received but not yet put away |

> **TIP:** Create a physical label for every bin location in the shop using the exact Bin Location ID as it appears in JobBOSS². This creates a one-to-one link between the physical label and the system record. When employees perform counts or receipts, they read the physical label and type exactly what they see into the system.

### Assigning Bin Locations to Part Records

After all bin locations are created in Table Maintenance, each Part Master record must be updated to reference its bin location(s).

**Navigation:** Inventory → Parts → [Select Part] → Bin Locations tab (or Inventory tab, depending on JobBOSS² version)

**Step-by-Step:**

1. Open the Part Master record for the item you want to assign.
2. Navigate to the **Bin Locations** tab or section.
3. Click **Add** to add a bin location assignment.
4. In the **Bin Location** field, click the lookup icon or type the bin location ID.
5. Select the appropriate bin from the list.
6. In the **Default** column or checkbox, mark one bin location as the default. The default bin is used automatically on receiving documents and pick lists unless overridden.
7. Optionally add a second or third bin location if the material is stored in multiple areas (e.g., a large steel bar may occupy both `RACK-A1` and `RACK-A2` if it is long).
8. Save the record.

### Multiple Bin Locations Per Part

A single part can be assigned to multiple bin locations. This is useful for:
- Overflow storage when the primary bin is full
- Different stock states (e.g., a receiving bin for incoming material and a production bin for material in use)
- Tracking remnants separately from full stock (assign both `RACK-A1` and `DROPS` to the same part record)

> **NOTE:** When issuing material to a job that has multiple bin locations, the user will be prompted to select which bin to pull from. Employees should always pull from the default bin first unless that bin is empty, in which case they pull from the secondary bin and update the system accordingly.

### Default Bin Location Assignment

The default bin location is the bin that will be pre-populated on:
- Receiving documents (where to put the material when it arrives)
- Pick lists and work orders (where to go to get the material)
- Physical count worksheets (which bin to count this part in)

Every part must have exactly one default bin location. Parts without a default bin cannot be received without manual bin entry, which increases the risk of receiving errors.

---

## 3.5 Part Master Setup for Material Control

The Part Master record is the foundational record for every item tracked in inventory. For Die-Mension, this means every material, consumable, cutting tool, and hardware item that will be purchased and stocked.

### Navigation

Main Menu → Inventory → Parts → New (for new part) or search for existing

### Required Fields — Field-by-Field Reference

**Part Number**
- Maximum visible length: 30 characters (displays on screen and most reports)
- Maximum typeable length: 50 characters (additional characters can be typed but will be truncated to 30 on purchase orders and most printed documents)
- **CRITICAL:** Part numbers longer than 30 characters will be cut off on purchase orders. Vendors will not see the full part number. Keep all part numbers at 30 characters or fewer.
- Recommended format for Die-Mension: Material grade + form + size, e.g., `D2-RND-1.250`, `H13-PLT-6X6`, `O1-RND-0.500`
- Once a part number is created and transactions exist, changing it requires careful coordination — see common mistakes in Section 3.14

**Description**
- Maximum length: 30 characters (this is a hard display limit in most views and on printed documents)
- Pack as much useful information into 30 characters as possible
- Example: `D2 TOOL STEEL RND 1.250 DIA` (28 chars — fits)
- Example: `H13 HOT WORK DIE STEEL PLT 6X6` (31 chars — will be truncated; remove one character)

**Unit of Measure**
- The single most important configuration decision on the Part Master aside from costing method
- Must reflect how the material is purchased, stocked, and issued
- Options: `EA` (each), `LB` (pounds), `FT` (linear feet), `IN` (inches), `PC` (piece), `PR` (pair)
- For tool steel in bar/round form, common options are `LB` (if tracking by weight) or `FT` (if tracking by length)
- See Section 3.10 for a detailed discussion of unit of measure choices for tool and die shops
- **WARNING:** The unit of measure on a Part Master that has open transactions should not be changed. The existing on-hand quantity is expressed in the current UOM, and changing it changes the meaning of that quantity without converting the number.

**Costing Method**
- Field location: Part Master → General tab → Costing Method dropdown
- Options: FIFO, LIFO, Standard, Average
- Select per the accountant-approved decision documented in Section 3.3
- For Die-Mension: Average Cost recommended for all material categories
- This field must be set before the first receipt

**Standard Cost (if Standard Cost method selected)**
- If using Standard Cost method, enter the standard cost per unit here
- This is the cost that will be used for all issues regardless of actual purchase price
- Must be reviewed and updated at least annually

**Lot Control Enable/Disable Toggle**
- Field location: Part Master → General tab → Lot Control checkbox (checkbox may be labeled "Lot Tracked" or "Track Lots" depending on version)
- Check this box for all materials requiring heat number traceability
- For Die-Mension: Check for all tool steels, carbide blanks, and any material with a mill cert requirement
- Do NOT check for consumables (coolant, rags, drill bits) where traceability is not required
- **Must be checked BEFORE the first receipt of the part**

**Reorder Point**
- Field location: Part Master → General tab or Inventory tab → Reorder Point field
- Enter the quantity at which the system should flag the part for repurchase
- Formula and calculation guidance: See Section 3.7
- Enter in the same unit of measure as the part record

**Economic Order Quantity (EOQ)**
- The quantity to purchase when reordering
- This is a suggestion, not a hard rule — it pre-fills the quantity on generated POs from the MRP screen
- Calculate based on typical purchase quantities from vendors
- For bar stock: often a full bundle or a standard cut length quantity

**Lead Time**
- Enter the number of days from PO placement to expected receipt
- Used in reorder point calculations and in the MRP forecast
- For common tool steels from domestic distributors: 10–21 days is typical
- For specialty grades or imported material: 30–60 days

**Primary Vendor**
- Link the primary vendor for this material to the part record
- When a PO is auto-generated from the MRP screen, the primary vendor is pre-populated
- Navigate to the Vendor tab on the Part Master and add the vendor record

**Bin Location Assignment**
- See Section 3.4 for detailed instructions
- Must be assigned after bin locations are created in Table Maintenance

**Stocking Flag / Inventory Flag**
- There is typically a checkbox on the Part Master labeled "Stock" or "Inventoried" or similar
- This flag must be checked for all materials that will be tracked in inventory
- Parts without this flag checked are treated as non-stock and will not appear in inventory reports or MRP
- **For Die-Mension: Check this flag on all materials. If a material is purchased and used, it should be tracked.**

**Part Class / Category**
- Assign each part to a category for reporting purposes
- Suggested categories for Die-Mension: TOOL-STEEL, CARBIDE, CUTTING-TOOLS, COOLANT, HARDWARE, GRINDING, MISC
- Categories are used to filter inventory reports and valuation reports

### Part Master Quick Reference Card for Die-Mension

| Field | Required? | Notes |
|---|---|---|
| Part Number | YES | Max 30 chars; use grade-form-size format |
| Description | YES | Max 30 chars; be descriptive |
| Unit of Measure | YES | LB or FT for bar stock; EA for discrete items |
| Costing Method | YES | Average Cost (per accountant approval) |
| Lot Control | YES for tool steels | Check BEFORE first receipt |
| Reorder Point | YES | Calculate per Section 3.7 |
| EOQ | Recommended | Set to typical purchase qty |
| Lead Time | Recommended | Enter in days |
| Primary Vendor | Recommended | Links to Vendor table |
| Bin Location | YES | Assign after bin locations created |
| Stocking Flag | YES | Must be checked to track inventory |
| Part Class | Recommended | For report filtering |

---

## 3.6 Lot/Heat Number Control Configuration

### Overview

Lot control in JobBOSS² creates a link between a specific batch of material (a "lot") and all transactions involving that material — receipts, issues, job completions, and shipments. For a tool and die shop, the lot number is almost always the heat number from the steel mill certification document (mill cert).

The heat number is the unique identifier assigned by the steel mill to a specific melt of steel. Every bar, round, or plate cut from that melt carries the same heat number, and the mill cert documents the chemistry (carbon content, chromium, vanadium, etc.), mechanical properties, and origin of that specific heat. When a customer audits Die-Mension or requests traceability documentation for a tooling deliverable, the question is: "Show me the mill cert for the material used in this tool." Lot control in JobBOSS² provides the system-based answer to that question.

### Enabling Lot Control on a Part Record

**Navigation:** Inventory → Parts → [Part Record] → General tab

1. Open the Part Master record for the material.
2. Locate the **Lot Control** checkbox (may be labeled "Lot Tracked," "Track by Lot," or "Lot/Serial Control" depending on version).
3. Check the box to enable lot control.
4. Save the record.

> **CRITICAL:** Lot control must be enabled BEFORE the first receipt of a part. If material has already been received without lot control enabled, those receipt records have no lot assignment. Enabling lot control on a part that already has receipts creates a situation where some inventory is lot-tracked and some is not — an auditor will view this as a traceability gap. For Die-Mension starting fresh, this is not a concern as long as the configuration is done before going live.

### What Happens at Receiving When Lot Control Is Enabled

When a purchase order receipt is processed for a lot-controlled part, JobBOSS² will present an additional field or prompt for the lot number before the receipt can be saved.

**Receiving Workflow with Lot Control:**

1. Open the PO in Purchasing.
2. Click **Receive** on the line item.
3. The receiving screen appears with standard fields (quantity, unit cost, etc.).
4. An additional **Lot Number** field appears (because the part has lot control enabled).
5. Enter the heat number from the mill certification. Type it exactly as it appears on the mill cert document — typically a 6–10 digit alphanumeric code.
6. Complete the other receiving fields.
7. Save the receipt.
8. The system now permanently links: **Heat Number → PO Receipt → Bin Location → On-Hand Quantity**

When that material is later issued to a job, the system records: **Job Number → Heat Number → Quantity Used**

This creates a complete chain: **Customer Order → Job → Material Issue → Heat Number → PO Receipt → Vendor → Mill Cert**

### For Tool and Die: Lot Number = Heat Number

Die-Mension should establish a shop policy that the lot number in JobBOSS² is always the heat number from the mill cert. Do not use purchase order numbers, internal batch numbers, or other identifiers as the lot number. The heat number is the universal identifier that connects the system record to the physical mill cert document.

> **SHOP POLICY:** When receiving any tool steel, the receiving employee must obtain the mill cert from the package before closing the receipt. The heat number on the mill cert is entered as the lot number in JobBOSS². The mill cert is then scanned and attached to the receipt record or the PO record in the document management module. If a mill cert is not available at the time of receipt, the receipt should be held at the RECEIVING bin location in a quarantine status until the mill cert is obtained.

### Multiple Heats Per Shipment — Known Limitation (JBCORE-I-128)

A common scenario in tool and die shops: a single purchase order line for "10 bars of D2 1.250 dia x 18 inches" is fulfilled by the distributor using bars from two or three different heats. The distributor sends two mill certs, one covering bars 1–6 (heat A) and one covering bars 7–10 (heat B).

**The problem:** In JobBOSS², a single PO line item receipt is typically associated with a single lot number. If you receive the entire quantity of 10 bars as a single receipt, you can only enter one lot number — you lose the ability to distinguish which bars came from which heat.

**Known Issue Reference:** This limitation is tracked as JBCORE-I-128 in some versions of the JobBOSS² issue tracker. Check with your JobBOSS² support contact for the current status of this enhancement.

**Workaround Options:**

*Option A — Separate Line Items on the PO (Preferred):*
When creating the PO, enter the expected material as separate line items if you know or suspect multiple heats will be supplied. At receiving time, receive each line item with its corresponding lot number.
- Limitation: You do not always know in advance how many heats will arrive.

*Option B — Separate Receipts Against the Same PO Line (Most Practical):*
When the shipment arrives and you discover multiple heats, receive as separate receipts against the same PO line — one receipt for the quantity from heat A (enter heat A's heat number as lot), and a second receipt for the quantity from heat B (enter heat B's heat number as lot).
- In JobBOSS², a PO line can typically be received against multiple times until the total received quantity equals the ordered quantity.
- This is the recommended workaround for Die-Mension.

*Option C — Sublot Numbers (If Available in Your Version):*
Some versions of JobBOSS² support sublot numbers or serial numbers within a lot. If this functionality is available, it can be used to distinguish individual bars within a heat. Check with your JobBOSS² support representative.

**Procedure for Die-Mension When Multiple Heats Are Present:**

1. Material arrives. Receiving employee opens the PO in JobBOSS².
2. Receiving employee physically separates bars by heat number using the mill certs.
3. Receives the first quantity against the PO line, entering the first heat number as the lot number. Save.
4. Receives the second quantity against the same PO line, entering the second heat number as the lot number. Save.
5. The PO line now shows total received quantity across both receipts.
6. Labels the physical bars with their respective heat numbers using permanent marker or adhesive tags.
7. Attaches both mill certs to the PO record or the respective receipt records in the document management module.

### Mill Cert Attachment Workflow

Attaching mill cert PDFs to the JobBOSS² record creates a system-based audit trail that satisfies AS9100 and ISO 9001 requirements for material traceability.

**Method 1 — Attach to the Receiving Record:**

1. After saving the receipt in JobBOSS², locate the document attachment functionality. In most versions, this is a paperclip icon or an "Attachments" button on the receiving record.
2. Click **Add Attachment** or equivalent.
3. Browse to the scanned mill cert PDF on the network drive or local drive.
4. Select the file and attach it.
5. The mill cert is now permanently linked to that specific receiving record, which is in turn linked to the lot number and part record.

**Method 2 — Attach to the Purchase Order Record:**

1. Open the PO in the Purchasing module.
2. Navigate to the Attachments section.
3. Attach the mill cert PDF.
4. This is less precise (the PO may cover multiple materials from different heats) but is acceptable if the PO is for a single line item with a single heat.

**Method 3 — File Folder on Network Drive with Systematic Naming:**

If the document attachment functionality in JobBOSS² is not reliable or if the file size limit prevents attachment, maintain a network folder with the following naming convention:

`\Mill Certs\[Year]\[PO Number]-[Heat Number]-[Material Grade].pdf`

Example: `\Mill Certs\2026\PO-2026-0412-A44821-D2.pdf`

This method requires that employees know to look in the network folder for mill certs. It is less integrated but workable as a fallback.

> **RECOMMENDATION:** For AS9100 compliance, Method 1 (attach directly to the receiving record) is preferred. If Die-Mension is pursuing or maintaining AS9100 certification, the traceability chain must be demonstrable from the part number to the heat number to the mill cert document in a single system query. Attachments to the receiving record provide this chain.

### Audit Trail for AS9100 / ISO 9001 Compliance

With lot control properly configured, JobBOSS² provides the following traceability chain that satisfies AS9100 clause 8.5.2 (Identification and Traceability):

- Customer order → Job number → Material requirements → Material issue record → Lot number (heat number) → PO receipt record → Vendor → Mill cert (attached document)

The **Lot Tracking Report** (see Section 3.13) can trace this chain in both directions: forward (from heat number to jobs) and backward (from job to heat number).

---

## 3.7 Reorder Point Calculation Guide

### Formula

The fundamental reorder point formula is:

**Reorder Point = (Average Daily Usage × Lead Time in Days) + Safety Stock**

When the on-hand quantity falls to or below the reorder point, the system flags the part for reorder. The goal is to ensure that the shop does not run out of material during the period when a new order is in transit.

### How to Determine Average Daily Usage for a Tool and Die Shop

Tool and die shops do not have the high-volume repetitive production patterns of a stamping plant. Usage of tool steel is project-driven and highly variable. A week with no D2 usage can be followed by a week where three large D2 round bars are consumed on a single die insert job.

**Practical Approach for Die-Mension:**

1. **Look back at purchasing history.** Pull the last 12 months of purchasing records for each material. Total the quantity purchased. Divide by 365 to get average daily usage.
   - Example: In 2025, Die-Mension purchased 480 lbs of D2 round stock. Average daily usage = 480 / 365 = 1.32 lbs/day.

2. **Look at open backlog.** If there are large jobs currently on the schedule that will consume significant material, factor those in. The historical average does not capture upcoming demand spikes.

3. **Round up generously.** For a small shop, the cost of carrying an extra bar of D2 on the shelf is far less than the cost of an emergency order and production delay. Use conservative (higher) estimates for average daily usage.

### Safety Stock Considerations

Safety stock is additional buffer inventory held above the minimum calculated level. For Die-Mension:

- **Supplier reliability:** If the primary steel distributor frequently ships short or late, increase safety stock.
- **Grade criticality:** For grades like D2 and H13 that are used in almost every die set, carry higher safety stock. For specialty grades ordered on demand (M4, CPM-10V), safety stock may be zero.
- **Minimum practical order quantity:** If the vendor sells in minimum quantities of one bar, the safety stock should be at least one full bar equivalent.
- **Recommended safety stock for common grades:** 5–7 days of average usage.

### Example Calculations

**Example 1: D2 Tool Steel Round Stock, 1.250" Diameter**

- Historical annual usage: 480 lbs
- Average daily usage: 480 / 365 = 1.32 lbs/day
- Lead time from primary supplier: 14 days (2 weeks)
- Safety stock: 5 days of usage = 1.32 × 5 = 6.6 lbs → round up to 10 lbs
- **Reorder Point = (1.32 × 14) + 10 = 18.5 + 10 = 28.5 lbs → round up to 30 lbs**
- Enter 30 in the Reorder Point field on the Part Master

**Example 2: H13 Hot Work Die Steel, Flat Plate 6" × 6" × 12"**

- Historical annual usage: 8 pieces
- Average daily usage: 8 / 365 = 0.022 pieces/day
- Lead time: 21 days (supplier stocks this but it's a specialty item)
- Safety stock: 1 piece (minimum practical)
- **Reorder Point = (0.022 × 21) + 1 = 0.46 + 1 = 1.46 → round up to 2 pieces**
- Enter 2 in the Reorder Point field

**Example 3: O1 Oil-Hardening Tool Steel Round, 0.500" Diameter**

- Historical annual usage: 120 lbs
- Average daily usage: 120 / 365 = 0.33 lbs/day
- Lead time: 10 days (locally stocked at distributor)
- Safety stock: 3 days = 0.33 × 3 = 1 lb
- **Reorder Point = (0.33 × 10) + 1 = 3.3 + 1 = 4.3 lbs → round up to 5 lbs**
- Enter 5 in the Reorder Point field

### Reorder Point Setup Worksheet for Die-Mension

Karen should complete this worksheet for all stocked materials before going live. A blank template is provided here:

| Part Number | Description | Annual Usage | Daily Usage | Lead Time (Days) | Safety Stock | Reorder Point | Enter in System? |
|---|---|---|---|---|---|---|---|
| | | | | | | | |
| | | | | | | | |
| | | | | | | | |

---

## 3.8 Purchase to Stock vs. Purchase to Job Workflows

Understanding the difference between these two purchasing workflows is fundamental to accurate inventory and job costing in JobBOSS². Using the wrong workflow results in either inflated inventory balances (purchased to stock when should be to job) or depleted inventory balances (purchased to job when should be to stock).

### Purchase to Stock (Receipt to Inventory)

**Definition:** Material is purchased for general inventory — it will be available to any job that needs it. The cost of the material becomes part of the inventory asset on the balance sheet. When the material is later issued to a job, the cost moves from inventory to job cost (work in process).

**When to Use:** Standard stocked materials that are kept on the shelf and used across multiple jobs. For Die-Mension: D2 round stock, H13 plate, cutting tools, hardware, grinding wheels, and coolant are all candidates for Purchase to Stock.

**Step-by-Step Workflow:**

1. **PO Creation:** A purchase order is created for a vendor. The PO line item references a Part Master record for a stocked item. No job number is entered on the PO line. The PO line has a "type" of Stock or Inventory (terminology varies by version).

2. **Vendor Ships Material:** The vendor ships the material with a packing slip and, for steel, a mill certification.

3. **Receiving Department Opens PO:** Navigate to Purchasing → Purchase Orders → search for the open PO by PO number or vendor name. Open the PO record.

4. **Select Receive on the PO Line:** Click the **Receive** button or right-click the line item and select **Receive**. The receiving entry screen opens.

5. **Enter Quantity Received:** Enter the actual quantity received in the **Qty Received** field. This may differ from the ordered quantity if the vendor shipped a partial order.

6. **Enter Unit Price if Different from PO:** If the vendor's invoice price differs from the PO price, enter the actual price in the unit cost field. This difference may generate a purchase price variance depending on system configuration.

7. **Assign Lot Number (if lot control enabled):** If the Part Master has lot control enabled, a Lot Number field will be present. Enter the heat number from the mill cert. See Section 3.6 for detail.

8. **Confirm Bin Location:** Verify the bin location pre-populated on the receiving record. Change it if the material is being placed in a different location than the default.

9. **Save the Receipt:** Click Save. The system performs the following actions simultaneously:
   - Increases the on-hand quantity of the part by the received quantity
   - Records the cost of the received material in the inventory ledger using the selected costing method
   - Creates a receiving transaction record linked to the PO and lot number
   - Decreases the on-order quantity by the received quantity
   - If average cost method: recalculates the average cost for the part

10. **Inventory On-Hand Increases.** The part is now available for issue to jobs.

**Result:** The material cost sits in inventory (an asset account on the balance sheet) until the material is issued to a job.

### Purchase to Job (Job-Specific Receipt)

**Definition:** Material is purchased specifically for a single job. The cost bypasses the inventory asset account entirely and posts directly to the job's cost record as a direct material cost. The material never appears in general inventory.

**When to Use:** Custom, one-time materials that are purchased for a specific job and have no reuse potential. For Die-Mension: an unusually large block of Inconel ordered for a specific aerospace die insert; a specialty carbide grade specified by the customer; an expensive custom casting ordered for a specific die set.

**Step-by-Step Workflow:**

1. **PO Creation with Job Reference:** A purchase order is created for a vendor. The PO line item references the job number. The line item type is "Job" or "Outside Process" (terminology varies by version). No Part Master record is required, though it can be referenced.

2. **Material Ships, PO is Received:** The receiving process is similar to Purchase to Stock, but the system behavior at posting is fundamentally different.

3. **Open PO and Click Receive:** Same navigation as Purchase to Stock.

4. **Enter Quantity and Price:** Same fields.

5. **Save the Receipt:** The system posts the cost of the material directly to the referenced job as a direct material cost. It does NOT increase inventory on-hand.

6. **Job Cost Increases Immediately.** The job now shows the material cost without the material ever passing through inventory.

**Result:** The material cost appears immediately on the job cost record. The inventory on-hand balance is unaffected.

> **WARNING — MOST COMMON MISTAKE AT DIE-MENSION:**
> If a toolmaker runs out of D2 and asks Karen to order "some D2 for the [specific job]," the instinct is to create a job-specific PO and receive it to that job. However, if D2 is a regularly stocked item, this bypasses inventory and may leave future jobs with no D2 on hand, but no system record showing it was all consumed. Always receive standard stocked materials to inventory (Purchase to Stock), even if the immediate trigger for the order was a specific job. Then issue the material from inventory to the job.

### Decision Matrix: Purchase to Stock vs. Purchase to Job

| Scenario | Purchase to Stock | Purchase to Job |
|---|---|---|
| D2 round bar for general use | YES | No |
| H13 plate for regular die work | YES | No |
| Custom carbide insert specified by customer for a single job | No | YES |
| Standard drill bits, taps, end mills | YES | No |
| Special ordered 4140 forgings for one die base, no future use | No | YES |
| Coolant and lubricants | YES | No |
| Material customer-supplied (consigned) | Neither — track separately | — |
| Outsourced heat treating for a specific job | No | YES (outside process) |

---

## 3.9 Issuing Material to Jobs (Allocation and Consumption)

Once material is in inventory, it must be issued to jobs to reflect actual consumption. This is how job costs are built up with accurate material costs and how inventory balances decrease to reflect usage.

### Understanding the Difference: Allocation vs. Issuance

**Allocation (Reservation):** Allocating material to a job reserves a quantity of that material for the job without physically removing it from inventory. The on-hand quantity does not change. The allocated quantity is flagged as "committed" or "reserved" and will appear in MRP calculations as demand that is already covered. This is a planning step.

**Issuance (Consumption):** Issuing material to a job records the actual physical removal of material from the bin and assignment to the job. The on-hand quantity decreases. The job cost increases by the cost of the issued material. This should happen when the material is physically pulled from the shelf and taken to the machine.

### Material Requirements on the Job Record

When a job is created or quoted, material requirements can be entered in the job record under the Materials or Bill of Materials section. This entry defines what materials are expected to be consumed by the job, in what quantities.

**Navigation:** Jobs → [Job Record] → Materials tab

For each material requirement:
1. Select the Part Master record (part number)
2. Enter the required quantity in the unit of measure of the part
3. Optionally enter the unit cost (will be populated from inventory cost)
4. Save

These material requirements drive MRP by creating "demand" for the material on the job's required date. They also allow the allocation step.

### Allocating (Reserving) Material to a Job

**Navigation:** Jobs → [Job Record] → Materials tab → Select the material line → Allocate

1. Open the job record.
2. Navigate to the Materials tab.
3. Select the material line you want to allocate.
4. Click the **Allocate** button (may also be accessed via right-click context menu).
5. The system checks the available (unallocated) on-hand quantity.
6. If sufficient quantity is available, the allocation is confirmed.
7. The quantity moves from "Available" to "Allocated/Committed" in the inventory record.
8. The on-hand total does not change, but the available-for-other-jobs quantity decreases.

> **TIP:** Run the Material Availability screen or MRP screen after allocating materials to verify that no other jobs are now short on the same material.

### Issuing (Consuming) Material to a Job

**Navigation Option A (From the Job):** Jobs → [Job Record] → Materials tab → Select line → Issue

**Navigation Option B (From Inventory Module):** Inventory → Transactions → Material Issue

**Step-by-Step from the Job Record:**

1. Open the job record.
2. Navigate to the Materials tab.
3. Select the material line item to be issued.
4. Click the **Issue** button.
5. The issue screen opens, pre-populated with the material, job number, required quantity, and default bin location.
6. Verify or adjust the **Quantity to Issue.** For partial issues, enter less than the full required quantity.
7. Verify the **Bin Location.** If pulling from a non-default bin, change this field.
8. If lot control is enabled: the system will show available lots (heat numbers) in inventory. Select the lot from which material is being pulled.
9. Click **Save / Post Issue.**
10. The following happens simultaneously:
    - On-hand inventory quantity decreases by issued quantity
    - If allocated, the allocated quantity is consumed
    - Job material cost increases by: issued quantity × current average cost (or FIFO layer cost, or standard cost — per costing method)
    - Transaction record is created linking: Job → Material → Quantity → Cost → Lot Number → Bin

### Effect on Inventory Quantities

After an issue transaction:
- **On Hand:** Decreases by issued quantity
- **Allocated:** Decreases by issued quantity (if previously allocated)
- **Available (On Hand minus Allocated):** May be unchanged if allocation was done first, or decreases if issue was done directly without prior allocation

### Effect on Job Cost

The material cost posted to the job is:

- **Average Cost method:** Issued quantity × current weighted average cost per unit at time of issue
- **FIFO method:** Issued quantity × cost of oldest layer in inventory
- **Standard Cost method:** Issued quantity × standard cost (regardless of actual cost in inventory)

This cost appears on the job cost summary as a direct material cost and is used to calculate job profitability.

### Returns to Stock If Material Is Unused

If material is issued to a job but the job is completed with material left over, the unused material should be returned to inventory to maintain accuracy.

**Navigation:** Inventory → Transactions → Material Return to Stock (or from the Job record → Materials tab → Return)

1. Enter the job number from which material is being returned.
2. Select the part being returned.
3. Enter the quantity being returned.
4. Specify the bin location where material will be returned.
5. If lot control enabled: re-enter the lot number (heat number) of the material being returned.
6. Save. On-hand increases, job cost decreases.

> **NOTE:** Returns to stock should be recorded the same day the material is returned to the shelf. Delays create discrepancies between the physical shelf and the system record.

### Partial Issues

Partial issues are common in a tool and die shop — you may issue 8 lbs of D2 now and issue the remaining 4 lbs required by the job later, once the first operation is complete. JobBOSS² handles partial issues natively.

Simply enter the partial quantity in the **Quantity to Issue** field at the time of each issue transaction. The Materials tab on the job will show issued vs. required quantity, making it easy to see how much has been issued versus how much is still needed.

---

## 3.10 Coil/Sheet/Bar Stock Considerations for Tool and Die

### Typical Material Forms at Die-Mension

Die-Mension, as a precision tool and die shop, will typically stock and use the following material forms:

- **Tool steel bar (round/square/hex):** D2, H13, O1, A2, S7, M2, 4140, P20 — purchased in standard lengths (typically 3 ft, 6 ft, or 12 ft) from steel distributors
- **Tool steel flat stock/plate:** D2 plate, H13 plate, S7 plate — purchased in standard sizes or custom-cut blanks
- **Carbide blanks:** Solid carbide rods and plates — purchased by length and diameter
- **Coil stock:** Occasionally, for shops that also manufacture progressive die stampings for testing
- **Drops and remnants:** Partial bars and plates left over after cutting operations

### Unit of Measure Selection for Bar and Plate Stock

This is a critical and often poorly handled decision in tool and die shops. The three main options are:

**Option A: Track by PIECE (EA)**
- Each bar or plate is counted as one piece regardless of length or weight
- Example: "10 EA of D2 round 1.250 dia" means 10 bars, regardless of how long each bar is
- **Problem:** After a bar is cut, you now have a remnant that is less than a full bar, but the system still shows 1 EA. If you do not have a way to close out the partial bar and issue a remnant record, your on-hand count becomes inaccurate quickly.
- **Recommended only if:** All bars are purchased and consumed whole, or if the cutting is done externally and the shop receives cut-to-size blanks.

**Option B: Track by WEIGHT (LB)**
- Inventory is tracked in pounds
- Receipts are entered as the weight of material received (from vendor's packing slip weight)
- Issues to jobs are entered as the weight consumed (requires scale in shop, or estimation from dimensions)
- Remnants are tracked by weight
- **Pros:** Most accurate for costing purposes, as steel is priced by the pound. Average cost per pound is easily compared to vendor pricing.
- **Cons:** Requires employees to weigh material or calculate weight from dimensions when issuing. Can be burdensome in a fast-moving shop.
- **Recommended for:** A-category tool steels where cost accuracy is critical.

**Option C: Track by LINEAR FEET (FT) or INCHES (IN)**
- Inventory is tracked by length
- Receipts are in feet or inches
- Issues are by the length cut
- **Pros:** Easy to measure — just run a tape on the bar. No weighing required.
- **Cons:** Cost calculation requires converting from a per-pound price to a per-foot price based on cross-section area and density — adds complexity to PO entry.
- **Recommended for:** Round bar stock of a single consistent diameter, where the per-foot cost can be calculated and kept stable.

**Recommendation for Die-Mension:**

Use a hybrid approach:
- Track high-value tool steels (D2, H13, M2, carbide) **by PIECE (EA)** if purchased as cut-to-size blanks from service center, or **by LB** if purchased in standard lengths and cut internally
- Track lower-value materials (hardware, coolant, grinding wheels) **by the natural purchase unit** (EA, GAL, EA respectively)
- Establish the UOM decision for each part before the first receipt and document it on the Part Master worksheet

### Tracking Remnants and Drops

Remnants are a constant reality in tool and die. A 12-inch bar of D2 is purchased and a 4-inch piece is cut off for a job insert. The remaining 8-inch piece goes back to the shelf. Without a systematic process, this 8-inch piece is:
- Not reflected in inventory (because the full bar was issued to the job)
- Not traceable to a heat number (because it was separated from the original material)
- A source of "ghost" material on the shelf that confuses physical counts

**Best Practice for Tracking Remnants at Die-Mension:**

1. Create a bin location specifically for remnants: `DROPS`
2. When a toolmaker cuts a piece from a bar, the remainder is taken to the DROPS bin.
3. At the end of the work order or at the end of the day, a material return transaction is created: the unused portion of the issued material (by weight or by piece, per UOM) is returned to inventory at bin location `DROPS` with the original heat number (lot number).
4. When a future job needs a small piece, the toolmaker checks the DROPS bin first before requisitioning a new full bar.
5. Periodically (monthly), review the DROPS bin for scraps that are too small to use. Issue those scraps to an overhead or scrap job to clear them from inventory.

> **NOTE:** Tracking drops by weight requires that the toolmaker weigh the remnant before it goes to the DROPS bin. If this is not practical, track by piece (1 EA = 1 remnant piece) and use the description to note approximate size. The exact size can be written on the piece with a paint pen and recorded in the description or notes field of the return transaction.

### Partial Bar Management

If Die-Mension tracks bar stock by PIECE (EA) and a bar is partially consumed:

- The job receives a full-piece issuance at job setup
- At job completion, the unused portion of the bar is returned to stock (material return transaction)
- The returned piece re-enters inventory as 1 EA with the original lot number
- The job is charged only for the portion used (the return reduces job cost by the average cost of 1 piece)

This results in a per-piece valuation that may not perfectly reflect the actual partial bar size, but it keeps the count and the lot number accurate.

---

## 3.11 MRP Walkthrough: Quick View Material Forecast Utility

JobBOSS² does not include a full MRP (Material Requirements Planning) engine in the traditional sense. However, the **Quick View Material Forecast Utility** (also called the **Material Forecast** or **Quick View MRP** screen depending on version) provides a practical planning tool that identifies material shortages based on current on-hand quantities, open POs, and material requirements from open jobs.

### Prerequisites for Effective MRP

Before running the Quick View Material Forecast, verify:

- [ ] On-hand quantities are accurate (physical count has been performed and entered)
- [ ] Reorder points are set on all stocked parts (Section 3.7)
- [ ] Open jobs have material requirements entered (Section 3.9)
- [ ] Outstanding POs are entered in the system (these represent on-order quantities that the forecast needs to know about)
- [ ] Lead times are entered on part records and/or vendor records

If any of these are missing, the forecast output will be incomplete or misleading. Garbage in, garbage out.

### Step-by-Step Navigation and Walkthrough

**Navigation:** Main Menu → Inventory → Quick View Material Forecast (may also be under Reports → Inventory → Material Forecast, depending on version)

**Step 1 — Open the Quick View Material Forecast Screen**

The screen will load and display a list of part records. By default, it typically shows parts that are either:
- At or below their reorder point, OR
- Have demand from open jobs that exceeds current available supply

**Step 2 — Review the Columns**

The screen displays the following columns for each part:

| Column | Description |
|---|---|
| Part Number | The part ID from the Part Master |
| Description | Part description |
| On Hand | Current physical on-hand quantity |
| On Order | Quantity on outstanding open POs not yet received |
| Required | Total quantity demanded by open jobs (from job material requirements) |
| Available | On Hand + On Order − Required |
| Shortage | Amount by which Available is negative (if any) |
| Reorder Point | The reorder threshold set on the Part Master |
| Suggested Order Qty | EOQ or calculated quantity to bring inventory to safe level |
| Primary Vendor | The primary vendor linked to the part record |

> **TIP:** A negative number in the Available or Shortage column means the shop is currently projecting a stockout for that material based on open job demand. These rows should be addressed immediately.

**Step 3 — Filter and Sort**

- Sort by the Shortage column (descending) to bring the most critical shortages to the top.
- Filter to show only parts with shortages if the full list is long.
- Review each shortage row and determine whether an immediate PO is needed.

**Step 4 — Select Items to Purchase**

- Use the checkbox or selection mechanism on each row to select the parts for which purchase orders should be generated.
- Review the suggested order quantity for each selected item. The suggested quantity is based on the EOQ set on the Part Master. Override it if necessary by typing a different quantity.
- Verify the primary vendor for each selected item.

**Step 5 — Generate PO Automatically from the Screen**

- Click the **Generate PO** button (or **Create Purchase Orders** or equivalent button — label varies by version).
- JobBOSS² will create draft POs for each selected item, grouped by vendor.
- Each PO will include the part number, description, order quantity, unit of measure, and the primary vendor.

**Step 6 — Review and Edit Generated POs Before Sending**

- The system will redirect you to the Purchasing module, or open the generated POs directly.
- Navigate to Purchasing → Purchase Orders and open each newly generated PO.
- Verify:
  - Vendor is correct
  - Part number and description are correct
  - Quantity is appropriate
  - Unit of measure is correct
  - Unit price is current (update if needed)
  - Delivery address and ship-to are correct
  - Required date is set
- Add any notes or special instructions for the vendor.
- Set the PO status to **Open** or **Sent** when ready.
- Send the PO to the vendor by email, fax, or EDI per normal purchasing procedures.

> **IMPORTANT:** POs generated by the Quick View Forecast are draft documents until reviewed and approved. Do not assume that generating a PO in the system means the vendor has been contacted. The PO must be transmitted to the vendor through normal channels.

---

## 3.12 Physical Inventory Process

### When to Run Physical Inventory

Physical inventory — a full count of all on-hand material quantities — should be performed:

- **Year-end:** Required for financial statement preparation. The balance sheet must reflect accurate inventory value.
- **Quarterly cycle counts:** Best practice for a shop of Die-Mension's size. Cycle counts divide the inventory into sections and count a portion each quarter rather than doing one big annual count. This reduces disruption.
- **Post-major-event:** After a major reorganization of the shop, after a suspected theft or loss, or after a system data import.
- **Before going live with the Material Control module:** This is a one-time startup physical count to establish the baseline on-hand quantities in the system.

> **NOTE FOR DIE-MENSION GOING LIVE:** The very first physical inventory is the most important one. Before the first receipt transaction is entered, a physical count of all materials currently on hand must be performed, entered into the system, and posted. This establishes the starting on-hand quantities and the starting inventory value. Without this baseline, all subsequent inventory reports will be wrong from day one.

### Physical Inventory Process — Step by Step

**Step 1 — Plan the Count**

- Identify all bin locations that need to be counted.
- Assign counting teams to specific bins or areas.
- Schedule the count during a period when no production transactions will occur (evening, weekend, or during a scheduled shutdown).
- Obtain counting sheets in advance.

**Step 2 — Navigate to Physical Inventory in JobBOSS²**

Navigation: Inventory → Physical Inventory (may also be under Inventory → Transactions → Physical Count, depending on version)

**Step 3 — Freeze Inventory (Prevent Transactions During Count)**

Before printing count sheets, freeze the inventory to prevent transactions from occurring while the count is in progress. A transaction posted during an active count will create a discrepancy between the count sheets and the system.

- In the Physical Inventory screen, select the option to **Freeze Inventory** or **Lock Inventory for Counting**.
- The system will warn that transactions cannot be posted while inventory is frozen.
- Confirm the freeze.

> **WARNING:** Once inventory is frozen, no receiving, no material issues, and no inventory adjustments can be posted until the freeze is lifted. Coordinate with all personnel to ensure no transactions are attempted during the count period.

**Step 4 — Print Count Sheets**

- From the Physical Inventory screen, select **Print Count Sheets** or **Print Worksheets**.
- Choose to sort by bin location (recommended) so that counters can move through the shop systematically without backtracking.
- Print count sheets for each counting area/team.
- The count sheets will show: Part Number, Description, Unit of Measure, Bin Location. The **Quantity on Hand** column will typically be blank or hidden so that counters are not influenced by the system quantity.

**Step 5 — Perform the Physical Count**

- Teams move through the shop, counting physical items in each bin location.
- Write the actual physical count quantity on the count sheet.
- If a part is found in a bin not listed on the count sheet, note it separately.
- If a part is found with no label, set it aside for identification.
- Double-count high-value items.
- Do not skip bins or estimate — every bin must be physically verified.

**Step 6 — Enter Counts into the System**

- Return to the Physical Inventory screen.
- Enter the count quantities from the count sheets into the system.
- The system will display the current book quantity (the frozen quantity) alongside the entered count quantity.
- Discrepancies are automatically calculated.

**Step 7 — Review and Investigate Discrepancies**

Before posting, review all significant discrepancies:
- Large shortfalls: Were items issued to jobs but not recorded? Were items lost, stolen, or scrapped?
- Large excesses: Were items received but not entered in the system? Were items returned from jobs without a return transaction?
- Investigate and resolve discrepancies above a threshold (e.g., any discrepancy greater than $50 in value should be explained before posting).

**Step 8 — Post Adjustments**

- When the count is verified and discrepancies are understood, click **Post Adjustments** or **Post Physical Count**.
- The system creates adjustment transactions for every bin and part where the count differs from the book quantity.
- On-hand quantities are updated to match the physical count.
- Inventory value is adjusted accordingly (value of adjustments posts to an inventory adjustment account in the general ledger — consult accountant for proper GL account mapping).

**Step 9 — Unfreeze Inventory**

- After all adjustments are posted, return to the Physical Inventory screen and select **Unfreeze Inventory**.
- Normal transactions (receiving, issues, etc.) can now resume.
- Confirm with all personnel that the system is back in normal operation mode.

**Step 10 — File the Count Documentation**

- Keep signed, dated count sheets on file for audit purposes.
- The count sheets serve as supporting documentation for the inventory adjustment entries in the general ledger.
- For AS9100 compliance, the physical inventory records should be retained per the document control schedule.

---

## 3.13 Key Reports for Material Control

The following reports are the primary tools for monitoring, auditing, and managing material inventory at Die-Mension. Navigation paths are approximate — exact menu location may vary by JobBOSS² version.

**Navigation to most reports:** Reports → Inventory → [Report Name]

---

### Transaction Register Report

**What it shows:** A chronological list of all inventory transactions within a selected date range — receipts, issues, adjustments, returns, and transfers. Each transaction shows: date, transaction type, part number, description, quantity, unit cost, extended cost, job number (if applicable), PO number (if applicable), and employee who posted the transaction.

**Use it to:** Audit inventory activity, investigate discrepancies, verify that specific transactions posted correctly, support accounting period-end review, and demonstrate traceability to auditors.

**Navigation:** Reports → Inventory → Transaction Register

---

### Stale Inventory Report

**What it shows:** Parts that have had no activity (no receipts, no issues) within a specified number of days. This identifies inventory that may be obsolete, excess, or forgotten on the shelf.

**Use it to:** Identify materials that have not been used in 12+ months and may be candidates for return to vendor, resale, or write-down. Reduce carrying costs of dead inventory.

**Navigation:** Reports → Inventory → Stale Inventory (may also be called Inactive Inventory Report)

---

### Inventory Valuation Report

**What it shows:** The total financial value of all on-hand inventory, broken down by part, with unit cost, on-hand quantity, and extended value. Typically shows totals by category/class.

**Use it to:** Support balance sheet preparation, verify that inventory value is consistent with prior period, monitor the total asset value of raw material inventory, and provide data for insurance purposes.

**Navigation:** Reports → Inventory → Inventory Valuation

> **NOTE:** Run this report before and after every physical inventory posting to confirm that the adjustment amounts are reasonable.

---

### Material Requirements Report

**What it shows:** All material requirements from open jobs — what materials are needed, in what quantities, for which jobs, by what date.

**Use it to:** Understand total demand for materials across all open jobs, identify materials that are needed but may not be in stock, and feed into the purchasing and MRP process.

**Navigation:** Reports → Inventory → Material Requirements (or Jobs → Material Requirements Report)

---

### Reorder Report

**What it shows:** Parts whose on-hand quantity is at or below the reorder point set on the Part Master. Shows on-hand, reorder point, and the shortage or trigger.

**Use it to:** Daily/weekly monitoring of inventory levels to drive purchasing decisions. This is a simpler version of the MRP screen — it shows reorder triggers based on quantity alone, without factoring in open job demand.

**Navigation:** Reports → Inventory → Reorder Report (or Inventory → Reorder Report)

---

### Physical Count Worksheets

**What it shows:** A formatted worksheet listing all parts and bin locations for use during a physical inventory count. The quantity on hand column is blank for counters to write in.

**Use it to:** Conduct the physical inventory count. Print sorted by bin location for counting efficiency.

**Navigation:** Inventory → Physical Inventory → Print Worksheets (or Reports → Inventory → Physical Count Worksheets)

---

### Open PO Report

**What it shows:** All purchase orders that have been issued but not fully received. Shows vendor, PO number, line items, ordered quantity, received quantity, and balance remaining.

**Use it to:** Monitor what material is on order and expected, verify on-order quantities in the MRP screen, follow up with vendors on overdue POs, and prevent duplicate ordering.

**Navigation:** Reports → Purchasing → Open Purchase Orders

---

### Receiving History Report

**What it shows:** All receiving transactions within a date range — what was received, from which vendor, on which PO, in what quantity, at what cost, and with what lot number.

**Use it to:** Trace material from a specific vendor receipt, audit receiving activity, verify that lot numbers were recorded at receipt, and support traceability investigations.

**Navigation:** Reports → Purchasing → Receiving History (or Reports → Inventory → Receiving History)

---

### Lot Tracking Report (Lot Control Must Be Enabled)

**What it shows:** The full traceability chain for a specific lot number (heat number). Shows: PO receipt, quantity received, quantity on hand, quantity issued, jobs that consumed the lot, quantities issued by job, and current remaining quantity.

**Use it to:** Answer the question "Where did all of heat number A44821 go?" in both forward and backward directions. Critical for customer traceability requests, AS9100 audits, and non-conformance investigations.

**Navigation:** Reports → Inventory → Lot Tracking (or Inventory → Lot Control → Lot Tracking Report)

> **IMPORTANT:** This report only produces meaningful output if lot control was enabled on the part and lot numbers were consistently entered at every receipt. A missing lot number on a single receipt creates a gap in the traceability chain.

---

## 3.14 Common Material Control Mistakes (With Fixes)

The following mistakes are commonly made in shops that are setting up or operating Material Control in JobBOSS² for the first time. For each mistake, the consequence and the fix are provided.

---

### Mistake 1: Not Setting Up Bin Locations Before the First Receipt

**What happens:** Material is received but there is no bin location to assign it to. The receipt either fails, is posted without a bin, or is posted to a generic or default bin that does not reflect where the material actually is. Future pick lists and count sheets are meaningless because nothing is in a defined location.

**Fix:** Before going live, complete Step 4 of the Prerequisites Checklist (Section 3.2). Create all bin locations in Table Maintenance. Assign bin locations to all Part Master records. Do not receive a single piece of material until this is done.

---

### Mistake 2: Selecting the Wrong Costing Method (or Leaving It at the Default)

**What happens:** JobBOSS² may default to a costing method (often Average Cost or Standard Cost depending on installation). If this default is not reviewed and confirmed with the accountant, it may not be the intended method. After receipts exist, changing the method is extremely difficult.

**How to detect:** Run the Inventory Valuation Report and compare the reported average unit cost to recent vendor invoices. If they do not reasonably align, the costing method may be wrong or cost data may be corrupt.

**Fix:** Before going live, complete Step 6 of the Prerequisites Checklist. Confirm the costing method with the accountant. Document the decision. If the wrong method was used and transactions already exist, contact JobBOSS² support — a data correction may be possible at significant effort and cost.

---

### Mistake 3: Not Enabling Lot Control Before the First Receipt

**What happens:** Tool steel is received without lot control enabled. Heat numbers are not recorded in the system. When a customer requests traceability documentation for a die set, there is no system record linking the material to a heat number. The shop must search paper records or mill certs manually.

**How to detect:** Open a Part Master record for a tool steel item. Check whether the Lot Control checkbox is checked. If it is not checked, check the receiving history — if any receipts exist without lot numbers, there is a traceability gap.

**Fix for future receipts:** Enable lot control on the Part Master immediately. All future receipts will require a lot number.

**Fix for past receipts:** You cannot retroactively add lot numbers to existing receipt records through normal system operation. Contact JobBOSS² support for options. In the interim, maintain paper records linking PO receipts to mill certs.

---

### Mistake 4: Purchasing to Job When Should Purchase to Stock

**What happens:** Stocked materials are repeatedly purchased job-by-job instead of to general inventory. The system never builds an accurate inventory balance. Each job-specific PO costs direct to the job (correct for that job), but the on-hand quantity never increases. The next job that needs the same material shows zero on hand even though there may be leftover material from the previous job on the shelf.

**How to detect:** Review recent POs. If they all have job numbers on the lines and the inventory on-hand for common materials is zero or near zero despite those materials physically being on the shelf, this mistake is occurring.

**Fix:** For all standard stocked materials, receive to inventory (Purchase to Stock). Then issue from inventory to each job that consumes the material. See Section 3.8 for the decision matrix.

---

### Mistake 5: Not Running MRP / Not Using the Quick View Forecast

**What happens:** Purchasing is driven entirely by toolmakers asking Karen to order material when they notice they are out. Emergency orders, rush shipping charges, and production delays accumulate. There is no proactive visibility into upcoming material needs.

**How to detect:** If the shop regularly places emergency orders with overnight or same-day shipping, the MRP process is not being used.

**Fix:** Run the Quick View Material Forecast at least weekly — ideally every Monday morning before the week begins. Review all shortage rows and generate POs for any items that are short. Set reorder points per Section 3.7 so that the system alerts before shortages occur.

---

### Mistake 6: Not Setting Reorder Points

**What happens:** The Quick View Material Forecast only shows items with open job demand shortages. Parts that are below a safe stocking level but have no immediate job demand are invisible. The shop appears to be fine until a job starts and the material is not there.

**How to detect:** Open Part Master records for key materials. Check the Reorder Point field. If it is zero or blank, reorder points have not been set.

**Fix:** Complete Step 8 of the Prerequisites Checklist. Calculate reorder points for all A and B category materials per the formula in Section 3.7. Enter them in the Part Master records.

---

### Mistake 7: Not Performing Physical Counts

**What happens:** System on-hand quantities drift further and further from physical reality over time due to unrecorded scrap, unrecorded issues, receiving errors, and other discrepancies. After 12-18 months without a physical count, the inventory valuation report may differ from actual inventory value by thousands of dollars. MRP decisions based on false on-hand quantities are unreliable.

**Fix:** Perform a full physical inventory count before going live (to establish baseline). Thereafter, perform a full count at year-end and cycle counts quarterly. See Section 3.12 for the full process.

---

### Mistake 8: Alpha Characters in Job Numbers (Breaks Data Collection Terminal Integration)

**What happens:** If Die-Mension uses or plans to use data collection terminals (bar code scanners, shop floor data collection devices) in conjunction with JobBOSS², job numbers with letters in them can cause integration failures. Many legacy data collection integrations expect purely numeric job numbers.

**How to detect:** If job numbers contain letters (e.g., "DIE-2026-001"), test the data collection terminal with a scan of that job number. If the terminal does not recognize it or throws an error, this is the problem.

**Fix:** Establish a job numbering convention that uses only numeric characters before going live. Example: six-digit sequential numbers starting at 100001. If job numbers with alpha characters already exist in the system, consult with JobBOSS² support and the data collection terminal vendor before proceeding.

---

### Mistake 9: Not Tracking Remnants and Drops

**What happens:** Material is issued to a job in full bar quantities. After cutting, a usable remnant is placed on a shelf but no return transaction is posted. The job cost is overstated (charged for more material than was consumed). Inventory on hand is understated (the remnant is not in the system). Future jobs either use the remnant without knowing its cost basis or heat number, or they order new material when perfectly good material is sitting on the DROPS shelf.

**Fix:** Establish the remnant tracking workflow described in Section 3.10. Create the DROPS bin location. Train all toolmakers on the return-to-stock procedure. Designate one person to audit the DROPS bin monthly and clear out scrap.

---

### Mistake 10: Not Attaching Mill Certs to Receiving Records

**What happens:** Mill certs are received with shipments and filed in paper folders or on someone's desk. When a customer requests a mill cert for a job completed six months ago, no one can find it. The paper has been misfiled, discarded, or is with a former employee. The shop cannot provide traceability documentation and risks losing the customer.

**Fix:** Establish the mill cert attachment workflow described in Section 3.6. Scan every mill cert at receiving and attach it to the receiving record in JobBOSS² immediately. Store the physical copy as a backup but treat the digital attachment as the primary record. Assign this task to the receiving employee as a step in the receiving procedure.

---

### Mistake 11: Not Reviewing the Open PO Report Before Placing Orders

**What happens:** Karen places a PO for D2 round stock because a toolmaker asks for it. An existing PO for D2 is already open from two weeks ago but not yet received. The new PO results in a double shipment. The shop now has more D2 than it can use in the near term, cash is tied up in excess inventory, and storage becomes a problem.

**Fix:** Before placing any PO for a stocked material, run or review the Open PO Report (Section 3.13) to check what is already on order. Alternatively, check the on-order quantity displayed in the Quick View Material Forecast, which factors in open POs.

---

### Mistake 12: Inconsistent Unit of Measure Across Related Parts

**What happens:** D2 1.250 round is set up as LB, but D2 1.500 round is set up as EA. When the Inventory Valuation Report totals D2 inventory, the quantities cannot be meaningfully combined because one is in pounds and one is in pieces. Comparisons across similar materials become impossible.

**Fix:** Establish a UOM convention by material category before creating Part Master records (see Section 3.10). Apply the convention consistently across all parts in a category. If UOM must be corrected after transactions exist, the correction is complex — contact JobBOSS² support.

---

## 3.15 Die-Mension Specific Configuration Recommendations

This section provides specific, actionable configuration guidance tailored to Die-Mension's operating environment as a precision tool and die shop in Brunswick, OH. These recommendations should be implemented as part of the initial Material Control setup.

### Recommended Costing Method: Average Cost

As discussed in Section 3.3, **Average Cost is the recommended costing method for all material categories at Die-Mension.**

Rationale specific to Die-Mension:
- Steel distributor pricing for common grades (D2, H13, O1) fluctuates with domestic steel market conditions. Die-Mension's purchasing volume does not allow for long-term fixed-price contracts, so prices vary from order to order.
- Average Cost smooths these fluctuations into job costs, preventing a single high-priced receipt from spiking the cost of jobs in that week.
- Die-Mension does not have a dedicated cost accountant available to maintain standard costs. Average Cost requires no ongoing maintenance.
- The accountant must formally confirm this selection before implementation.

**Action:** Enter "Average Cost" in the Costing Method field on every Part Master record. Do not leave any stocked parts at the system default without confirming that the default is Average Cost.

### Recommended Bin Structure

Organize bin locations by material type and physical location in the shop. The following bin structure is recommended as a starting point for Die-Mension:

**Steel Storage Area (Primary Rack):**

| Bin ID | Contents |
|---|---|
| `RACK-A1` | D2 Tool Steel — Round Bar, Diameters 0.500" – 1.000" |
| `RACK-A2` | D2 Tool Steel — Round Bar, Diameters 1.001" – 2.000" |
| `RACK-A3` | D2 Tool Steel — Flat Stock / Plate |
| `RACK-B1` | H13 Hot Work Steel — Round and Flat |
| `RACK-B2` | O1 Oil Hardening Steel — Round Bar |
| `RACK-B3` | A2 Air Hardening Steel — Round and Flat |
| `RACK-C1` | S7 Shock Resisting Steel |
| `RACK-C2` | M2 High Speed Steel |
| `RACK-C3` | 4140 / P20 / General Purpose Steel |
| `COIL-STORAGE` | Flat Plate and Oversized Stock |
| `DROPS` | Remnants and Cut-Off Pieces |

**Secondary Storage Areas:**

| Bin ID | Contents |
|---|---|
| `CARBIDE-SHELF` | Carbide Blanks — Rods and Plates |
| `TOOL-CRIB` | Cutting Tools — End Mills, Drills, Taps, Inserts |
| `WHEEL-SHELF` | Grinding and Dressing Wheels |
| `HARDWARE-BIN` | Fasteners, Dowel Pins, Springs, Shoulder Bolts |
| `COOLANT-RACK` | Cutting Fluids, Coolant Concentrate, Way Oil |
| `SHELF-C1` | Miscellaneous Small Parts |
| `RECEIVING` | Incoming Material — Not Yet Put Away |
| `QUARANTINE` | Material on Hold — Awaiting Inspection |

Label every bin with a physical label matching the Bin ID exactly as it appears in JobBOSS². Use a label printer for legibility and durability.

### Lot Control: Required for All Tool Steels

**Die-Mension must enable lot control on all tool steel part records.** This is non-negotiable for the following reasons:

1. **Customer requirements:** Automotive and industrial customers purchasing tooling from Die-Mension increasingly require material traceability documentation at the die build level.
2. **AS9100 / ISO 9001 compliance:** If Die-Mension is pursuing or maintaining a quality management system certification, clause 8.5.2 (Identification and Traceability) requires the ability to identify and trace materials throughout production.
3. **Failure investigation:** If a die insert fails prematurely, the ability to trace the material heat number allows investigation of whether the steel met specification. Without lot control, this investigation cannot be performed systematically.
4. **Competitive differentiation:** The ability to provide a mill cert traceability package with a tooling delivery is a value-added service that larger automotive tier suppliers increasingly expect.

**Parts that must have lot control enabled (check the Lot Control checkbox on Part Master):**

- All tool steel grades: D2, H13, O1, A2, S7, M2, 4140 (hardened), P20, and any other steel used in die construction
- Carbide blanks
- Any material supplied to a customer specification or covered by a purchase specification

**Parts where lot control is optional or not required:**

- Standard cutting tools (end mills, drills, taps) — unless a specific customer requires it
- Hardware (fasteners, dowel pins) — unless customer-specified hardware grade requirements exist
- Consumables (coolant, grinding wheels, way oil)

### Reorder Points: Lead Time Based on Steel Supplier Network

Common lead times from regional steel distributors serving the Brunswick, OH area (e.g., Olympic Steel, Service Center Ohio distributors, Metals Depot, Specialty Steel Supply) are as follows:

| Material Type | Typical Lead Time | Notes |
|---|---|---|
| D2, O1 common sizes | 7–14 days | Stocked locally by most distributors |
| H13 common sizes | 10–21 days | Some sizes may require mill order |
| A2, S7 common sizes | 14–21 days | Less commonly stocked locally |
| M2 High Speed Steel | 14–30 days | May require distributor order |
| Specialty / exotic grades | 30–60 days | Often mill order; plan accordingly |
| Carbide blanks | 7–21 days | Depends on grade and size |

Use these lead times as inputs to the reorder point formula in Section 3.7.

**Minimum recommended safety stock for Die-Mension:**

- D2 round stock (all common sizes): 1 full bar minimum; 2 bars preferred
- H13: 1 full bar/block of most common size used
- O1: 1 full bar of most common size used
- Carbide: 2 blanks of each size in current active use
- Cutting tools (end mills, drills): 2 of each size in active use

### Suggested Material Categories for Part Classification

Assign every Part Master record to one of the following categories when setting up the part records. These categories will be used to filter inventory reports, track valuation by material type, and organize the MRP screen.

| Category Code | Category Name | Contents |
|---|---|---|
| `TOOL-STEEL` | Tool Steel | D2, H13, O1, A2, S7, M2, 4140, P20 and all other ferrous die construction materials |
| `CARBIDE` | Carbide | Carbide rod blanks, plate blanks, inserts |
| `CUTTING-TOOLS` | Cutting Tools | End mills, drill bits, taps, reamers, boring bars, turning inserts |
| `COOLANT` | Coolant and Lubricants | Cutting fluid concentrates, coolant, way oil, spindle oil, rust preventive |
| `HARDWARE` | Hardware | Fasteners (screws, bolts, nuts), dowel pins, shoulder bolts, springs, retaining rings, die springs |
| `GRINDING` | Grinding Wheels | Grinding wheels (surface, cylindrical, ID), dressing sticks and tools |
| `MISC` | Miscellaneous | Items that do not fit other categories; review periodically to re-classify |

### Initial Setup Priority Order for Die-Mension

Given that the Material Control module is completely unconfigured, Karen should implement the setup in the following priority order to reach a functional state as quickly as possible:

**Priority 1 — Complete in Week 1 (Before Any Live Transactions):**
1. Meet with accountant — confirm costing method (Average Cost)
2. Create all bin locations in Table Maintenance
3. Enable lot control on all tool steel Part Master records
4. Assign bin locations to all Part Master records
5. Set costing method on all Part Master records

**Priority 2 — Complete in Week 2:**
6. Perform physical inventory count and enter starting on-hand quantities
7. Post physical inventory adjustments to establish baseline
8. Calculate and enter reorder points for all A-category materials (tool steels)

**Priority 3 — Complete in Week 3:**
9. Enter reorder points for B-category materials (carbide, cutting tools)
10. Verify vendor records have lead times entered
11. Link primary vendors to part records
12. Train all shop employees on the material issue and return procedure
13. Establish the mill cert scanning and attachment workflow

**Priority 4 — Ongoing:**
14. Run Quick View Material Forecast weekly
15. Perform monthly audit of DROPS bin
16. Perform quarterly cycle counts
17. Review and update reorder points as usage patterns change
18. Perform full physical inventory count at year-end

> **FINAL NOTE FOR AI ASSISTANTS GUIDING KAREN:**
>
> If Karen asks you to help her perform any action in the Material Control module, always first ask:
>
> 1. Has the costing method been confirmed with the accountant and entered on the Part Master?
> 2. Are bin locations set up and assigned?
> 3. Is lot control enabled on the part in question (if it is a tool steel)?
> 4. Has the initial physical inventory baseline been established?
>
> If the answer to any of these is "no," redirect Karen to complete the prerequisite before proceeding with the transaction. The order of operations in this module is not flexible — shortcuts taken now will create data integrity problems that are very costly to correct later.

---

*End of Section 3: Material Control Deep Dive*

*Next Section: Section 4 — Purchasing Module Configuration and Workflow*

*Previous Section: Section 2 — System-Wide Table Maintenance and Initial Configuration*

---

**Document Control Information**
- Document Title: JobBOSS² Master Reference Manual — Die-Mension Corporation
- Section: 3 — Material Control Deep Dive
- Status: DRAFT — For Review by Karen and Die-Mension Accountant
- Location: `C:\Users\gritm\die-mension\company\company\sops`
- Last Updated: February 2026
```

---

Here is a summary of what was written and how it is structured:

**File produced:** Section 3 of the JobBOSS/JobBOSS² Master Reference Manual, written in full Markdown, covering approximately 1,400+ lines of professional technical documentation.

**Structure delivered:**

- **3.1** — Overview explaining what Material Control does, the consequences of skipping configuration, and the critical costing method decision
- **3.2** — Nine-item ordered prerequisites checklist with a signed checkpoint for Karen
- **3.3** — Full four-method costing comparison table (FIFO, LIFO, Standard, Average) with a critical warning block and a specific recommendation for Die-Mension (Average Cost) with documented rationale
- **3.4** — Bin location setup with step-by-step navigation, a full naming convention table (19 bins), and assignment instructions
- **3.5** — Part Master field-by-field reference including the 30/50 character limit warning on part numbers, with a quick reference table
- **3.6** — Lot/heat number control including the JBCORE-I-128 multi-heat limitation, a three-option workaround, mill cert attachment workflow, and AS9100 traceability chain
- **3.7** — Reorder point formula with three worked examples (D2, H13, O1) and a blank calculation worksheet
- **3.8** — Purchase to Stock vs. Purchase to Job with full step-by-step workflows for each and a decision matrix table
- **3.9** — Material allocation vs. issuance distinction, step-by-step issue and return procedures, and partial issue handling
- **3.10** — UOM choices for bar/plate stock, remnant/drop tracking procedures, and partial bar management
- **3.11** — Quick View MRP walkthrough with column definitions and a prerequisite checklist
- **3.12** — Full physical inventory process in ten steps including freeze/unfreeze procedure
- **3.13** — Nine key reports with descriptions and navigation paths
- **3.14** — Twelve common mistakes, each with consequence explanation and fix procedure
- **3.15** — Die-Mension-specific recommendations including costing method rationale, bin structure table, lot control requirements, lead time reference table, material categories, and a four-priority-week implementation sequence


---

No relevant skill applies here. This is a technical documentation writing task for a JobBOSS/JobBOSS² manufacturing ERP manual. I will produce the full Markdown content now.

```markdown
# SECTION 4: LOT AND SERIAL NUMBER TRACKING DEEP DIVE

---

## 4.1 Overview: Why Lot and Serial Tracking Matter for Tool and Die

Lot and serial number tracking is not a feature that most small shops enable on day one. It requires discipline, consistent operator behavior, and upfront configuration effort. For a precision tool and die shop like Die-Mension, that investment pays returns across multiple business-critical areas.

### 4.1.1 Heat Number Traceability Requirements from Customers

Automotive OEMs, Tier 1 suppliers, aerospace primes, and defense contractors routinely require that the shop producing their tooling be able to demonstrate which specific heat of steel was used in a given die or fixture component. This requirement is not academic — it is contractual. Purchase orders and supplier quality agreements for these industries will commonly include language such as:

> "Supplier shall maintain material traceability records sufficient to identify the melt heat of all metallic raw materials incorporated into the supplied items. Records shall be retained for a minimum of [7/10/15] years."

If Die-Mension cannot produce that evidence upon request, the result may be a customer audit finding, a corrective action request (CAR), or loss of approved supplier status.

### 4.1.2 Mill Certification (Cert) Documentation Chain

Every heat of tool steel produced at a steel mill is accompanied by a mill test report (MTR), also called a mill certificate or cert. This document certifies:

- The chemical composition of the heat (carbon, chromium, vanadium, molybdenum percentages, etc.)
- The mechanical properties of the heat (hardness range, tensile strength, yield strength where applicable)
- The heat number assigned by the mill
- The mill name, date of production, and applicable material specification (e.g., AISI D2, ASTM A681, AMS 2300 series)

The documentation chain is: **Mill → Service Center → Die-Mension → Customer**. If Die-Mension receives steel from a service center (e.g., Metals Supermarkets, Alro, Metal Express), the service center should be passing through the original mill cert. Die-Mension must then retain that cert and be able to link it to specific inventory receipts and jobs.

JobBOSS lot control, when properly implemented, provides the database backbone for this chain.

### 4.1.3 AS9100D and ISO 9001:2015 Traceability Requirements

ISO 9001:2015 Clause 8.5.2 requires that when traceability is a requirement, the organization shall:
- Control unique identification of outputs
- Retain documented information necessary to enable traceability

AS9100D (used in aerospace) extends this significantly, requiring that organizations maintain a documented process for managing traceability of products, including materials, and that records demonstrate the linkage from raw material through all processing steps to the final product delivered.

Even if Die-Mension is not currently AS9100D certified, many aerospace customers will audit for AS9100D-aligned practices as a condition of supplier approval. Implementing lot tracking now positions Die-Mension for future certification or customer approval with those primes.

### 4.1.4 Material Certification Requirements for Tool Steels

Different tool steels carry different certification requirements:

| Steel Grade | Common Application at Die-Mension | Key Cert Requirements |
|---|---|---|
| D2 (1.2379) | Progressive die punches and die sections | Chemical comp, hardness range, MTR with heat number |
| H13 (1.2344) | Hot work tooling, die casting inserts | Chemical comp, charpy impact, MTR |
| A2 | General punching and blanking | Chemical comp, MTR |
| O1 | Gage blocks, low-production tooling | Chemical comp, MTR |
| S7 | Impact-resistant punches, shock loads | Chemical comp, Charpy, MTR |
| M2 | HSS punches, high-speed operations | Chemical comp, hardness, MTR |
| P20 | Plastic injection mold bases | Chemical comp, hardness, MTR |
| Carbide (various) | Wear-critical inserts | Grade cert, transverse rupture strength |

For each of these, the heat number on the MTR is the primary traceability identifier. JobBOSS lot numbers should be set equal to the heat number from the MTR to create an unambiguous, one-to-one link.

### 4.1.5 Liability and Warranty Traceability

When a progressive die fails prematurely in a customer's press line and causes scrap or line downtime, one of the first questions from the customer's quality team will be: "What was the material?" If material was substandard — wrong grade, wrong heat treatment, or contaminated heat — that may be the root cause. Die-Mension must be able to demonstrate that the correct material was used, received to cert, and properly documented. Without lot tracking, this defense is not possible. With lot tracking, Die-Mension can produce a complete material genealogy report.

### 4.1.6 Die-Mension Specific Context: Progressive Die Tooling

Progressive dies are high-value, long-service-life tooling. A single progressive die set for a customer stamping line may represent $50,000 to $500,000 in value and may serve in production for five to twenty years. Punches, die sections, pilots, and stripper plates are replaced as they wear. Each replacement component must be traceable to its material source. When customers renegotiate contracts or move production, the ability to provide complete traceability records for the existing tooling can be a significant value differentiator for Die-Mension.

---

## 4.2 Lot Control vs. Serial Control: Key Differences

Understanding the distinction between lot control and serial control is essential before configuring either in JobBOSS. They solve different problems and apply to different points in the production and inventory workflow.

| Feature | Lot Control | Serial Control |
|---|---|---|
| What it tracks | A batch of material or parts sharing a common identity | An individual, uniquely identified piece |
| Identifier example | Heat number 7854321-K on 500 lbs of D2 bar stock | Finished part serial number SN-0042 |
| Quantity tracked | Many units under one lot number | One unit per serial number |
| Primary use at Die-Mension | Raw material receipt by heat number | Finished tool or fixture component tracking |
| When to enable | Before the FIRST receipt transaction for the part | Before the FIRST production run for the part |
| JobBOSS location | Part/Material Master → Lot Control field | Part Master → Serial Control option (may require module) |
| Audit trail | Which jobs consumed which lots; current quantity by lot | Which machine produced the piece, which operator, when |
| Customer-facing use | "Your die section came from heat 7854321-K" | "Part serial SN-0042 was produced on [date] by [operator]" |
| Reporting | Lot Balance, Lot History, Lot Detail reports | Serial History, Serial Genealogy reports |
| Complexity | Moderate — requires consistent lot number entry at receiving | Higher — requires per-piece serial number assignment during production |

### 4.2.1 When to Use Lot Control at Die-Mension

Enable lot control on any part or material where:
- A heat number, batch number, or supplier lot number is provided at receipt
- Customer contracts or quality plans require material traceability
- The material is a critical steel grade used in tooling components
- The material has a limited shelf life or certification expiration date (e.g., cutting fluids with SDS lot numbers, adhesives, shims with plating certs)

### 4.2.2 When to Use Serial Control at Die-Mension

Enable serial control on any finished part or sub-assembly where:
- Customer purchase orders require serialized parts
- Internal quality records must be linked to a specific individual piece
- Warranty or repair tracking requires identifying which specific unit was shipped
- The item is a major assembly (a complete die set, a complete fixture)

### 4.2.3 Using Both Together

It is possible and often necessary to use both lot control and serial control on related items:
- The raw material (D2 steel bar) is lot-controlled by heat number
- The finished punch made from that bar is serial-controlled by part serial number
- The job links the lot number of the input material to the serial number of the output part
- This creates a complete forward and backward traceability chain

---

## 4.3 Enabling Lot Control on a Part Record (Step-by-Step)

This procedure must be followed exactly and in the correct sequence. The most common mistake — enabling lot control after receiving has already occurred — creates data integrity problems that are very difficult to correct.

### Prerequisites

Before beginning this procedure, confirm:
- You have administrative access to the Part/Material Master in JobBOSS
- No receiving transactions have yet been posted for this part
- You have the correct part number (or you are creating a new part record)
- You know what format the lot number will use (recommended: use the heat number exactly as printed on the mill cert)

### Procedure

**Step 1: Navigate to the Part/Material Master**

From the JobBOSS main menu, navigate to:
`Inventory` → `Part/Material Master`

Alternatively, from the top navigation bar, use the search function and type the part number if you already know it.

**Step 2: Open or Create the Part Record**

- If the part already exists: locate it using the search/filter function. Search by part number, description, or commodity code.
- If this is a new material: click `New` (or equivalent button) to create a new part record. Enter all required fields: Part Number, Description, Unit of Measure, Part Type (Raw Material), and any customer or commodity classification fields.

**Step 3: Locate the Lot Control Setting**

Within the part record, look for one of the following depending on your JobBOSS version:

- JobBOSS (legacy): Look for a tab labeled `Inventory` or `Control` within the part record. The lot control option will appear as a checkbox labeled `Lot Controlled` or `Track by Lot`.
- JobBOSS² (cloud): Navigate to the `Inventory` tab or `Tracking` section within the part record. The field may be labeled `Lot Tracking`, `Lot Control`, or appear as a dropdown with options `None`, `Lot`, `Serial`.

**Step 4: Enable Lot Control**

- Check the `Lot Controlled` checkbox, OR
- Select `Lot` from the tracking dropdown

Do not click Save yet. Review the record to ensure all other fields are correct before saving.

**Step 5: Review Related Settings**

While in the part record with lot control enabled, confirm the following related settings are appropriate:

| Setting | Recommended Value | Notes |
|---|---|---|
| Unit of Measure | LBS or FEET or EA (as appropriate) | Tool steel bar stock: LBS or INCHES |
| Bin Location | Assigned steel storage location | E.g., "STEEL-RACK-A" |
| Costing Method | Standard or Actual | Actual cost recommended for traceability |
| Minimum Quantity | Set appropriate reorder point | Optional but useful |
| Default Supplier | Primary steel supplier | Speeds up PO creation |

**Step 6: Save the Record**

Click `Save`. Verify the save was successful. The lot control indicator should now display as active/enabled on the part record.

**Step 7: Verify the Setting**

Close the part record and reopen it. Confirm that lot control is still showing as enabled after saving. Some JobBOSS configurations have required a full logout/login before lot control settings take effect on new transactions — if you encounter unexpected behavior at receiving, perform a full logout and login.

**Step 8: Document the Change**

Record the following in your quality records or change log:
- Part Number
- Part Description
- Date lot control was enabled
- Who enabled it
- Reason (e.g., "Customer XYZ contract requirement, PO #12345")

> **CRITICAL WARNING — Read Before Proceeding:**
>
> Enabling lot control AFTER material has already been received and posted to inventory does NOT retroactively assign lot numbers to existing on-hand quantities. You will have a split inventory condition where some quantity carries a lot number and some does not. JobBOSS will flag un-lotted inventory differently from lotted inventory, and some reports will show discrepancies that cannot be reconciled without manual journal-style adjustments. In a customer audit or ISO audit, a split inventory condition for a lot-controlled material is a finding.
>
> **The only correct time to enable lot control is BEFORE any receiving transactions are posted for that part.**
>
> If you must enable lot control on a part that already has on-hand inventory with no lot number, consult with your JobBOSS administrator. The recovery procedure requires creating a manual inventory adjustment to zero out the un-lotted quantity and re-entering it as a new receipt with an assigned lot number. This is a controlled, documented procedure — do not attempt it without administrative guidance and a backup of your database.

---

## 4.4 Heat Number Tracking Workflow for Tool Steel (Die-Mension Specific)

This section documents the complete, end-to-end workflow for receiving tool steel with heat number tracking into JobBOSS and maintaining full traceability from receipt through job consumption. This workflow applies to all tool steels purchased by Die-Mension: D2, H13, A2, O1, S7, M2, P20, carbide grades, and any other material received with a mill certification.

### 4.4.1 Phase 1: Purchase Order Creation

**Step 1:** Navigate to `Purchasing` → `Purchase Orders` → `New Purchase Order`.

**Step 2:** Select the Vendor (steel supplier). If the supplier is not in the system, it must be added to the Vendor Master first.

**Step 3:** Add a line item for the steel:
- Part Number: Select the lot-controlled part number from the Part Master
- Description: Should auto-populate from Part Master (e.g., "D2 TOOL STEEL BAR 1.5 x 1.5 x 12 FT")
- Quantity: Enter quantity in the unit of measure assigned to the part (LBS or PIECES or FEET)
- Unit Price: Enter current price per pound or per piece
- Due Date: Enter expected delivery date

**Step 4:** If ordering multiple steel grades or sizes on the same PO, add separate line items for each. This simplifies receiving and lot assignment.

**Step 5:** In the PO Notes or Special Instructions field, include a note: "Mill certification required with delivery. Provide heat number on cert and shipping documentation."

**Step 6:** Save and release the PO. Print or email the PO to the supplier.

> **Best Practice:** Some Die-Mension customers (particularly Tier 1 automotive) require that mill certs be reviewed and approved before the material is released to production. If this applies, add a note on the PO to hold material in a "HOLD" bin until cert review is complete. Establish a bin location named "INCOMING-HOLD" specifically for this purpose.

### 4.4.2 Phase 2: Material Arrival and Pre-Receiving Inspection

**Step 1: Physical Receipt at the Dock**

When the steel delivery arrives:
- Count the pieces received against the packing slip
- Check that bar sizes and grades match the PO
- Confirm that a mill certification document (paper or digital) accompanies the shipment
- If no mill cert is present: **Do not accept the material.** Place in a designated "HOLD — CERT PENDING" location and contact the supplier immediately. Document the discrepancy.

**Step 2: Identify the Heat Number**

On the mill certification, locate the heat number. It will typically appear:
- In a field labeled "Heat No.", "Heat Number", "Melt No.", or "Cast No."
- Near the top of the cert document
- As an alphanumeric code, e.g., "7854321-K", "A1234B", "MH-22-0987"

Write the heat number on the packing slip in permanent marker. This prevents confusion during the data entry step.

**Step 3: Check for Multiple Heats**

Examine the mill cert and the bars themselves. Many steel service centers ship from multiple heats within a single order. Bars may be stamped or tagged with heat numbers individually. Count how many distinct heat numbers appear in this shipment.

- If ONE heat number: proceed to Phase 3 as a single receiving transaction
- If MULTIPLE heat numbers: see Section 4.4.4 (Multiple Heats Per Shipment)

**Step 4: Scan or Copy Mill Cert to Digital Format**

If the cert is paper only, scan it to PDF immediately upon receipt. Name the file using the convention:

`HEAT-[HeatNumber]-MILLCERT-[YYYYMMDD].pdf`

Example: `HEAT-7854321K-MILLCERT-20250315.pdf`

Store the PDF in the shared network folder designated for mill certs (e.g., `\\die-mension-server\MIllCerts\`) AND attach it to the receiving record in JobBOSS (covered in Step 6 of Phase 3).

### 4.4.3 Phase 3: Receiving in JobBOSS

**Step 1:** Navigate to `Purchasing` → `Purchase Orders` → find the open PO for this delivery.

**Step 2:** Open the PO. Locate the line item for the steel being received.

**Step 3:** Click `Receive` against the PO line (button label may vary by version: "Receive", "Receive Against PO", "Add Receipt").

**Step 4:** A receiving screen will open. Enter:
- Quantity Received: Enter the actual quantity received (which may be less than, equal to, or — with supplier authorization — greater than PO quantity)
- Date Received: Confirm today's date or enter the correct date
- Receiving Dock: Select the receiving location or bin

**Step 5: Lot Number Entry**

Because lot control is enabled on this part, the system will either:
- Automatically display a "Lot Number" field that is required before the receipt can be posted, OR
- Prompt with a dialog box requiring lot number entry

In the Lot Number field, **enter the heat number from the mill cert exactly as it appears on the cert document.**

Do not abbreviate, truncate, or modify the heat number. If the cert shows "7854321-K", enter "7854321-K". Exact matching is critical for audit trail integrity.

> **Note on Lot Number Format:** JobBOSS lot number fields are typically alphanumeric and support hyphens, periods, and some special characters. If the heat number contains characters that the system rejects, document the exact heat number in the Notes field and use a simplified version as the lot number — then note both values in your quality records.

**Step 6: Attach the Mill Certificate**

After entering the lot number but before posting, attach the mill cert PDF to the receiving record:

- Look for an `Attachments`, `Documents`, or paperclip icon on the receiving screen
- Click it to open the attachment dialog
- Navigate to the scanned PDF file
- Attach it with the filename convention: `HEAT-[HeatNumber]-MILLCERT-[YYYYMMDD].pdf`
- Add a description: "Mill Certificate for Heat [HeatNumber]"

If the attachment function is not available on the receiving screen in your version of JobBOSS, attach the cert at the part record level using the same lot number as a reference, and maintain a parallel folder structure on the network server.

**Step 7: Enter Bin Location**

Specify which bin location in the shop the steel will physically be stored in. Recommended bin naming conventions for Die-Mension:

| Bin Name | Contents |
|---|---|
| STEEL-RACK-A | Primary D2 and A2 bar stock |
| STEEL-RACK-B | H13, S7, O1 bar stock |
| STEEL-PLATE | Flat plate stock, all grades |
| INCOMING-HOLD | Material awaiting cert review or receiving |
| DROPS-D2 | D2 remnants and drops below full-bar usable length |
| DROPS-MISC | Miscellaneous steel drops/remnants |
| QUARANTINE | Non-conforming material, awaiting disposition |

**Step 8: Post the Receipt**

Click `Post` or `Save and Post`. The system will:
- Create an inventory transaction: increase on-hand quantity for this part
- Record the lot number (heat number) associated with this quantity
- Record the PO it was received against
- Record the date, user, and quantity
- Update the bin location inventory balance

**Step 9: Verify the Receipt**

Navigate to `Inventory` → `Part/Material Master` → open the part record. View the `Inventory` tab or `Lot Detail` view. Confirm:
- On-hand quantity increased by the received amount
- The lot number appears correctly as a sub-record under the on-hand quantity
- The lot is associated with the correct bin location

### 4.4.4 Multiple Heats Per Shipment

This is a known limitation of JobBOSS receiving workflows (referenced in the user community as JBCORE-I-128). When a single delivery contains material from two or more distinct heats, the standard single-transaction receiving flow cannot capture all heats in one step with distinct lot numbers.

**Option A — Recommended: Separate Receiving Transactions Per Heat**

1. Complete the full receiving process (Steps 1–9 above) for the first heat number, entering only the quantity associated with that heat.
2. Go back to the PO and initiate a second receiving transaction against the same PO line.
3. Enter the quantity for the second heat and enter the second heat number as the lot number.
4. Attach the relevant portion of the mill cert (or a cert that covers multiple heats) to each receiving transaction separately.
5. Repeat for any additional heats in the shipment.

This approach creates separate lot records for each heat, each with its own quantity, and maintains full traceability.

**Practical Example:**

Delivery contains 5 D2 bars: 3 bars tagged heat 7854321-K and 2 bars tagged heat 8902211-M.
- Transaction 1: Receive 3 bars (or equivalent weight), Lot = "7854321-K", attach cert for heat 7854321-K
- Transaction 2: Receive 2 bars (or equivalent weight), Lot = "8902211-M", attach cert for heat 8902211-M

**Option B: Separate PO Lines Per Anticipated Heat**

If the order is large enough and the supplier can be asked in advance to confirm how many heats will be shipped, create separate PO lines for each expected heat at PO creation time. This is rarely practical for standard tool steel purchases but may be feasible for special-order materials.

**Option C: Single Transaction, All Under One Lot (Not Recommended)**

Receive all material under one lot number. This loses heat-level traceability for the second and subsequent heats. Only use this option when:
- The customer has expressly confirmed that heat-level traceability is not required
- The material is for internal use tooling not subject to customer traceability requirements
- You document the decision and your reasons in the job quality record

> **WARNING:** Do not use Option C for any material that will be used in tooling for automotive, aerospace, defense, or any customer with a quality agreement requiring heat traceability. Option C will result in an audit non-conformance if that customer reviews your material records.

### 4.4.5 Phase 4: Material Issue from Lot-Controlled Inventory to a Job

When a machinist or material handler issues steel from inventory to a specific job, the lot control system ensures the specific heat number is captured on the job.

**Step 1:** Navigate to the job record in `Job Management` or `Shop Order`.

**Step 2:** Access the `Material` or `Components` section of the job.

**Step 3:** When issuing a lot-controlled material, the system will display a list of available lot numbers for that part (grouped by bin location and quantity).

Example display:
```
Part: D2 1.5 x 1.5 BAR
  Lot 7854321-K   |  Bin: STEEL-RACK-A  |  On Hand: 47.3 LBS
  Lot 8902211-M   |  Bin: STEEL-RACK-A  |  On Hand: 22.8 LBS
```

**Step 4:** Select the specific lot number (heat number) to issue from. Follow FIFO (first in, first out) unless a specific heat is required.

**Step 5:** Enter the quantity to issue.

**Step 6:** Post the material issue transaction.

The result: The job record now permanently shows which lot number (heat number) was issued to it. The on-hand quantity of that lot decreases by the issued amount.

### 4.4.6 Phase 5: Responding to a Customer Traceability Inquiry

When a customer calls or sends a formal corrective action inquiry asking "What heat of steel was used in job number [XXXXX]?", the response process in JobBOSS is:

**Step 1:** Open the job record.

**Step 2:** Navigate to the `Material Transactions` or `Component History` section.

**Step 3:** The system shows all material issues to this job, including the lot number (heat number) for each lot-controlled material.

**Step 4:** Record the heat number. Navigate to `Inventory` → `Lot Detail` or `Lot History Report` for that lot number.

**Step 5:** The lot detail shows:
- Which PO the lot was received on
- The date of receipt
- The vendor who supplied it
- Any attachments (mill cert PDF)
- All jobs that consumed material from this lot

**Step 6:** Open the mill cert attachment. Export or print it.

**Step 7:** Provide the customer with:
- The heat number
- A copy of the mill cert
- The receiving date and PO reference
- The job numbers that consumed that heat

Total time to respond to a traceability inquiry, once the system is properly configured: 5–15 minutes. Without lot tracking: hours to days, and possibly impossible if paper records are not perfectly maintained.

---

## 4.5 Lot Tracking Reports

JobBOSS provides several standard reports that support lot tracking and traceability audits. Knowing which report to use for each situation is critical for efficient responses to customer inquiries and internal audits.

### 4.5.1 Lot Detail Report

**Purpose:** Shows every transaction — receipts, issues to jobs, transfers between bins, returns, and adjustments — for a single specified lot number.

**How to Access:**
- Navigate to `Inventory` → `Reports` → `Lot Detail`
- Or navigate to the specific lot number record and select `Print` / `View Transactions`

**Input Parameters:**
- Lot Number (required)
- Date Range (optional — defaults to all time)
- Part Number filter (optional)

**Output Includes:**
- Each transaction line with date, type, quantity, job number (if applicable), user who entered it, and current balance
- Net quantity on hand at end of report period

**Primary Use Case:** Responding to a customer asking "Where did lot [X] go?" — shows all consumption.

### 4.5.2 Lot Balance Report

**Purpose:** Shows current on-hand quantity broken down by lot number, bin location, and part number.

**How to Access:**
- Navigate to `Inventory` → `Reports` → `Lot Balance` or `Inventory by Lot`

**Output Includes:**
- Part Number, Part Description
- Each lot number associated with that part
- On-hand quantity per lot
- Bin location per lot
- Receipt date of lot
- Aging (how long the lot has been in inventory)

**Primary Use Case:** Physical inventory verification by heat number; identifying aged material that may require re-certification before use.

### 4.5.3 Lot History Report

**Purpose:** Complete chronological audit trail from initial receipt of a lot through all subsequent transactions.

**How to Access:**
- Navigate to `Inventory` → `Reports` → `Lot History`

**Output Includes:**
- All information from Lot Detail Report
- Vendor and PO reference for the original receipt
- Any quality holds or dispositions applied to the lot
- Traceability chain to consuming jobs

**Primary Use Case:** Full customer audit packages; ISO and AS9100D audit evidence; root cause investigations.

### 4.5.4 Material Usage by Lot (Lot Consumption by Job)

**Purpose:** Shows which jobs consumed material from a specified lot number, or alternatively, which lot numbers were consumed by a specified job.

**How to Access:** This report may appear under different names depending on JobBOSS version:
- `Inventory` → `Reports` → `Lot Usage`
- `Job Management` → `Reports` → `Material Traceability`
- In some versions, this is a custom report that must be built using the report writer

**Output Includes:**
- Job number, Job description, customer, date
- Quantity of the specified lot issued to each job
- Remaining balance after consumption

**Primary Use Case:** When a heat of steel is found to be non-conforming AFTER it has been partially consumed — identifies all jobs that used any portion of the bad lot so that affected parts can be flagged or recalled.

### 4.5.5 Running Audit-Ready Traceability Packages

For formal customer audits or corrective action responses, prepare a package that includes:
1. **Lot Detail Report** for the lot in question
2. **Purchase Order copy** for the receiving transaction
3. **Mill Certificate PDF** attached to the receiving record
4. **Job records** for all consuming jobs (job traveler printouts or job summary reports)
5. **Shipping records** showing where the finished parts went (if relevant)

This five-document package satisfies the traceability requirements of ISO 9001:2015, AS9100D, IATF 16949, and most customer-specific quality requirements.

---

## 4.6 Serial Control Module (for Finished Parts)

Serial control is a distinct capability from lot control. Where lot control tracks a quantity of identical items under a shared identifier, serial control tracks individual unique pieces — each with its own history.

### 4.6.1 When Serial Control Applies at Die-Mension

Serial control is appropriate for:
- Complete progressive die assemblies when the customer requires a die serial number
- Individual high-value components (die shoes, large punches) that will be maintained, refurbished, or replaced over time
- Fixture assemblies that have scheduled calibration or maintenance intervals
- Any part where a customer purchase order explicitly assigns a serial number to the part

Serial control is generally NOT needed for:
- Raw material (use lot control instead)
- Low-value consumable tooling (standard punches, retainers, springs)
- Production scrap or drops

### 4.6.2 Enabling Serial Control on a Part Record

The procedure is identical in structure to enabling lot control (Section 4.3) with the following differences:

**Step 1:** In the Part Master, locate the tracking field.

**Step 2:** Instead of selecting `Lot`, select `Serial`.

**Step 3:** Decide on the serial number assignment method:
- **Manual Assignment:** A user enters the serial number during production. Gives the shop control over the serial number format.
- **Auto-Assignment:** JobBOSS automatically generates the next sequential serial number. Convenient but requires configuration of the starting number and format.

**Step 4:** Set the serial number format/prefix if using auto-assignment (e.g., "SN-" prefix with 4-digit sequential suffix → SN-0001, SN-0002, etc.).

**Step 5:** Save the record before any production orders are created for this part.

> **CRITICAL WARNING:** As with lot control, enabling serial control after production has already begun creates inventory tracking problems. Parts already in inventory will have no serial number, and parts produced after enabling serial control will have serial numbers. This split condition is difficult to reconcile. Enable serial control before the first production run.

### 4.6.3 Serial Number Assignment During Production

When a work order or job is created for a serially controlled part:

1. During the production process, when the operator reports completion at the data collection terminal or in the desktop application, the system will prompt for the serial number.
2. The operator either enters the serial number manually or accepts the system-generated number.
3. The serial number is permanently linked to: the job it was produced on, the work center, the operator, the date, and the quantity completed (always 1 for serial-controlled parts).

### 4.6.4 Serial Number History and Reporting

For each serial number, JobBOSS maintains:
- Date manufactured
- Job number on which it was produced
- Work center(s) it passed through
- Operator(s) who worked on it
- Material lot numbers (heat numbers) consumed during its production (if the input materials are lot-controlled)
- Shipping history: which customer shipment it was included in, on which date

This creates a complete cradle-to-grave history for every serialized part Die-Mension produces.

### 4.6.5 Customer-Facing Serial Certification

When a customer requests a Certificate of Conformance (C of C) for a serialized part, JobBOSS serial history provides the data for the certificate body:
- Part number and description
- Serial number
- Customer PO number
- Production date
- Work order number
- Applicable specifications met
- Material heat number(s) used in production (from lot control linkage)
- Operator or inspector certification signature

This level of documentation satisfies Tier 1 automotive, aerospace prime, and MIL-SPEC customer C of C requirements.

---

## 4.7 Coil and Bar Stock Tracking Considerations

Tool steel at Die-Mension arrives primarily as bar stock (round bars, square bars, flat bars) or plate. The following considerations apply specifically to tracking partial consumption of bar stock by heat number.

### 4.7.1 Receiving Full Bars Under One Lot

When 10 bars of D2, all from heat 7854321-K, are received:
- Receive the total weight (e.g., 187.5 LBS) as one receipt transaction
- Assign lot number = 7854321-K
- All 187.5 LBS is tracked as one lot

### 4.7.2 Partial Issue to Jobs

When the first job requires a 6-inch length from one of those bars:
- Issue the calculated weight (e.g., 3.2 LBS) from lot 7854321-K to the job
- JobBOSS reduces the lot balance: 187.5 LBS → 184.3 LBS remaining
- The job record shows: 3.2 LBS of lot 7854321-K issued

When the second job requires material, it also draws from lot 7854321-K, further reducing the balance. This continues until the lot is exhausted or only drops/remnants remain.

### 4.7.3 Remnant and Drop Tracking

When a bar is partially consumed and the remaining piece is too short for a new job but too valuable to discard:
- The remaining piece should be physically moved to a designated DROPS bin (e.g., "DROPS-D2")
- In JobBOSS, perform a bin-to-bin inventory transfer: move the remnant quantity from "STEEL-RACK-A" to "DROPS-D2"
- The lot number (heat number) carries with the quantity — traceability is maintained on the drop
- Label the physical bar with the heat number and current weight/dimensions

**Recommended physical label for drops:**
```
MATERIAL: D2 TOOL STEEL
HEAT NO: 7854321-K
CERT: ON FILE (RECV 2025-03-15)
SIZE: ~1.5 x 1.5 x 4.75 APPROX
WT: ~1.1 LBS
BIN: DROPS-D2
```

### 4.7.4 Scrapping Exhausted Lots

When a lot is fully consumed (zero remaining), JobBOSS will show it as zero balance. The lot record and all its transaction history remain in the database for audit purposes — it is not deleted. The lot history is permanently available for traceability inquiries even after all material is consumed.

When a drop is too small or otherwise not usable:
- Create an inventory adjustment to reduce the lot quantity to zero
- In the adjustment notes, enter: "Drop scrapped — below minimum usable size — [date] — [initials]"
- Move the physical piece to the scrap bin

### 4.7.5 Unit of Measure Consistency

A common source of lot tracking errors at shops receiving bar stock is inconsistency between the unit of measure on the PO, the unit of measure on the part master, and how operators count pieces vs. weight.

**Recommended approach for Die-Mension:**

| Material Form | Recommended UOM | Notes |
|---|---|---|
| Round bar, square bar | LBS | Allows partial issue by weight |
| Flat bar, plate | LBS or SQ FT | LBS preferred for traceability |
| Pre-cut blanks (exact size) | EA (each) | When each blank is a known weight |
| Sheet material | SQ FT or LBS | Depends on customer unit requirements |
| Carbide rod/blank | EA or LBS | Carbide often priced per piece |

Once the unit of measure is set and receiving has begun under that UOM, do not change it without an inventory adjustment to zero and a fresh opening balance. Changing UOM mid-stream on an active lot creates calculation errors in inventory balances.

---

# SECTION 5: SHOP FLOOR DATA COLLECTION DEEP DIVE

---

## 5.1 Overview: What Data Collection Provides and Why It Matters

Shop floor data collection (DC) is the operational backbone of job costing accuracy, real-time work-in-process (WIP) visibility, and scheduling feedback in JobBOSS. Without it, JobBOSS operates on estimates only — estimated hours, estimated costs, estimated completion percentages. With a functioning data collection system, every hour of labor is reported in near-real-time, and management has an accurate picture of where every job is in the production workflow at any given moment.

### 5.1.1 What Data Collection Captures

At minimum, a properly configured shop floor data collection system at Die-Mension will capture:

- **Who** is working (Employee ID)
- **What** they are working on (Job Number and Operation/Routing Step)
- **Where** they are working (Work Center)
- **When** they started and when they finished (timestamps)
- **How many pieces** they produced (quantity good)
- **How many pieces were scrapped** (quantity rejected)
- **Whether they were in setup or run mode** (time type)
- **What material was consumed** (optional, for material-intensive operations)

### 5.1.2 The Business Value of Data Collection for Die-Mension

| Without Data Collection | With Data Collection |
|---|---|
| Job costing uses estimated hours only | Job costing reflects actual hours |
| No real-time WIP visibility | Management can see active jobs at any moment |
| Cannot identify bottlenecks until job is done | Bottlenecks visible in real time at work center level |
| Cannot calculate accurate labor efficiency | Labor efficiency reportable by employee, work center, job |
| Estimating accuracy never improves | Actual vs. estimated reports enable estimating refinement |
| Customer "where is my job?" requires walking the floor | Job status query available instantly in system |
| Cannot identify which operator produced a specific part | Full operator-level accountability possible |
| Scheduling is based on guesswork | Scheduling uses real data on job progress |

For a shop the size of Die-Mension, the difference between estimated and actual labor on a custom tooling job can easily be 20–40% of total job cost. If actual hours are consistently higher than estimated hours, Die-Mension is either underpricing jobs or absorbing losses. Data collection is what makes this visible before the loss becomes a pattern.

### 5.1.3 Realistic Expectations for Small Shop Data Collection

Data collection implementation is not a one-day event. Expect:
- 2–4 weeks for hardware setup, terminal configuration, and testing
- 1–2 weeks for employee training on the clock-in/clock-out process
- 4–8 weeks before data quality is reliable enough for management reporting
- 3–6 months before the data collection habit is embedded in shop culture

The biggest variable is not the software — it is operator compliance. The most common reason data collection fails at small shops is not technical; it is cultural. Operators are accustomed to not reporting their time in real time, and changing that habit requires consistent management enforcement.

---

## 5.2 Data Collection Hardware Options

Choosing the right hardware for each workstation is as important as the software configuration. Hardware that is not ruggedized for a machine shop environment will fail quickly. Hardware that is too far from the work area will not be used.

### 5.2.1 Hardware Options Comparison

| Hardware Type | Best Use Case | Estimated Cost | Pros | Cons |
|---|---|---|---|---|
| Dedicated shop floor terminal (purpose-built) | High-volume production floor, fixed stations | $800–$2,000 per unit | Ruggedized, touchscreen, designed for shop environment | Higher upfront cost |
| Repurposed desktop PC with standard monitor | Office area adjacent to shop, receiving desk | $0–$300 (if repurposing existing) | Familiar interface, easy to replace, supports keyboard entry | Not ruggedized, not suitable near coolant/chips |
| Repurposed PC with industrial touchscreen monitor | Fixed machine area with overhead mount | $200–$600 for screen | Large display, easy touch input, survives light shop environment | Monitor arms/mounts add cost and complexity |
| Tablet (iPad, Android, Surface) + DCMobile app | Mobile supervisors, inspection stations | $300–$800 per device | Portable, modern interface | WiFi dependency, app reliability issues (see 5.2.3), not ruggedized |
| Smartphone + DCMobile app | Very small shops, minimal stations | $0 (use existing phones) | No hardware cost | Small screen, app reliability issues, not rugged |
| Barcode scanner (keyboard-wedge USB) | Any station as a supplement to PC input | $20–$80 per scanner | Very reliable, inexpensive, plug-and-play | Requires manual keyboard entry for some fields |
| Barcode scanner (Bluetooth) | With tablet or mobile setup | $30–$150 per scanner | Wireless flexibility | Battery dependency, pairing issues |

### 5.2.2 Confirmed Working Hardware (Die-Mension Community References)

The following hardware types have been confirmed by the JobBOSS user community to work reliably with the desktop and terminal data collection interfaces:

**Barcode Scanners:**
- USB HID keyboard-wedge scanners from Amazon (brands: Inateck, Eyoyo, NetumScan) — $20–$50, plug-and-play, no driver installation required
- Refurbished Motorola Symbol scanners (LS2208, DS6607) — available on eBay for $15–$40, handle 1D and 2D barcodes, extremely durable
- Zebra DS2208 (new) — reliable, reads Code 128, Code 39, QR, and Data Matrix — $80–$120

**Terminals and Displays:**
- Any Windows PC running Chrome or Edge browser (for JobBOSS² cloud) or the JobBOSS desktop client
- 10-inch or larger industrial touchscreen displays with VESA mounting — look for IP54-rated units for near-machine use
- Fanless mini-PCs (e.g., Intel NUC-style units) work well for fixed station terminals — no moving parts, tolerate vibration better than desktop towers

**What NOT to Use:**
- Consumer iPads or Android tablets in areas with metalworking fluids or heavy chip generation — the ports will corrode
- Standard inkjet-printed barcode labels in areas with coolant exposure — labels degrade; use laminated or thermal-transfer printed labels

### 5.2.3 Known DCMobile App Issues (Current as of 2025–2026)

The DCMobile application (JobBOSS mobile data collection app for iOS and Android) has documented reliability issues that Die-Mension management should be aware of before deploying it:

| Issue | Severity | Status |
|---|---|---|
| App crashes when attempting to view job attachments on iPhone | High | Approximately 30% success rate — unresolved |
| Camera functionality broken after rebranding update | High | Reported by multiple users — under investigation |
| Login failures after some software updates | High | Cases open 1+ month without resolution as of report date |
| No ability to set a default scanner action | Medium | Ideas Portal: DCMOBILE-I-1 — not yet implemented |
| Attachment viewing not yet implemented on Android | Medium | Requested 2+ years ago: DCMOBILE-I-10 — still pending |
| Slow sync/lag on high-latency WiFi connections | Low-Medium | Managed by ensuring strong WiFi coverage in shop |

**Recommendation for Die-Mension:**

Deploy DCMobile only for time entry (clock in/clock out) and basic job status queries. Do not rely on DCMobile for:
- Viewing job attachments, blueprints, or traveler documents
- Camera scanning of barcodes (use a dedicated keyboard-wedge scanner instead)
- Any operation that requires consistent reliability

For operators who need to view drawings or job instructions at their workstation, use a fixed PC or dedicated terminal with a full browser rather than DCMobile.

---

## 5.3 Collection Terminal Configuration (Step-by-Step)

Every physical data collection station — whether a dedicated terminal, a shop PC, or a tablet — must have a corresponding Collection Terminal record in JobBOSS. Without this configuration, the terminal cannot be used for data collection.

### 5.3.1 Before You Begin

Confirm the following before creating terminal records:
- All physical hardware is installed and connected to the network
- JobBOSS is accessible from the physical device (test by logging in to the main application)
- You have a naming convention established for terminals (see recommended naming below)
- You know which work center each terminal will primarily serve
- Your job numbering format is confirmed (CRITICAL — see warning below)

### 5.3.2 Recommended Terminal Naming Convention for Die-Mension

| Terminal ID | Location | Default Work Center |
|---|---|---|
| MILL-1 | Haas VF-2 Vertical Mill | CNC-MILL |
| MILL-2 | Bridgeport Knee Mill | MANUAL-MILL |
| GRIND-1 | Surface Grinder #1 (Okamoto) | SURF-GRIND |
| GRIND-2 | Surface Grinder #2 | SURF-GRIND |
| EDM-1 | Wire EDM (Fanuc) | WIRE-EDM |
| EDM-2 | Sinker EDM | SINKER-EDM |
| LATHE-1 | CNC Lathe | CNC-LATHE |
| BENCH-1 | Assembly/Bench Work Area | BENCH |
| INSP-1 | Inspection/QC Station | INSPECTION |
| OFFICE-1 | Office — Supervisor/Admin | OFFICE |

Use short, descriptive, consistent IDs. Avoid spaces in terminal names — use hyphens instead.

### 5.3.3 Creating a Collection Terminal Record

**Step 1:** Navigate to:
`Table Maintenance` → `Collection Terminals`

In some JobBOSS versions, this may be under:
`Setup` → `Table Maintenance` → `Collection Terminals`
or
`Administration` → `System Tables` → `Data Collection Terminals`

**Step 2:** Click `New` to create a new terminal record.

**Step 3:** Complete the following fields:

| Field | Value | Notes |
|---|---|---|
| Terminal ID / Name | e.g., "MILL-1" | Must be unique across all terminals |
| Description | e.g., "Haas VF-2 CNC Mill Station" | Human-readable name |
| Default Work Center | Select work center from dropdown | Must already exist in Work Center master |
| Prompt for Employee | Yes (recommended) | Ensures per-operator tracking |
| Allow Multiple Active Operations | Yes | Required if operators run >1 machine |
| Job Number Format | Numeric only | See critical warning below |
| Auto Clock-Out at Shift End | Optional | Configure if using shift-based scheduling |
| Idle Timeout | Optional | Auto-logs out terminal after inactivity period |
| Printer Assignment | Assign shop printer if available | For printing labels or job tickets from terminal |

**Step 4:** Save the terminal record.

**Step 5:** Repeat for each physical terminal location.

**Step 6:** Test each terminal by logging in with an employee ID, entering a test job number, and confirming the data collection screen opens correctly.

> **CRITICAL WARNING — Numeric Job Numbers Required for Terminals:**
>
> Collection Terminal records in JobBOSS Table Maintenance are configured to accept numeric-only job numbers. If Die-Mension uses job numbers that contain alpha characters — such as "M-2024-001", "JOB-ABC-123", or "TOOL-2025-047" — those job numbers CANNOT be entered at data collection terminals using the terminal interface. Operators will receive an error or the field will simply reject the non-numeric characters.
>
> This is not a bug that will be fixed — it is a design characteristic of the terminal entry mode.
>
> **Mandatory Recommendation:** Configure JobBOSS to auto-generate sequential, purely numeric job numbers (example: 10001, 10002, 10003, continuing to 99999 and beyond). This ensures 100% compatibility with terminal data entry and barcode scanning.
>
> If your current job numbering already uses alpha characters, consult with your JobBOSS administrator before go-live on data collection. A migration to numeric-only job numbers may require a planned transition period.

### 5.3.4 Associating a Physical Device with a Terminal Record

In JobBOSS² (cloud version):
- The physical device accesses the data collection interface through a URL
- On the physical device, bookmark or pin the URL: `https://[yourcompany].jobboss2.com/datacollection/terminal?id=MILL-1`
- Set that bookmark as the browser homepage on the device

In JobBOSS (desktop/legacy version):
- Install the JobBOSS client on the terminal device
- Configure the startup shortcut to open directly to the data collection module for the assigned terminal ID
- Consider creating a dedicated Windows user profile on the terminal device that auto-launches the data collection screen at login (prevents operators from accessing other parts of the system)

---

## 5.4 Employee Setup for Data Collection

Every individual who will use the data collection system must have an employee record in JobBOSS. Shared employee IDs are a compliance risk and an accountability failure — enforce one ID per person without exception.

### 5.4.1 Creating Employee Records

**Step 1:** Navigate to:
`Table Maintenance` → `Employees`

**Step 2:** Click `New` to create a new employee record.

**Step 3:** Complete the following fields:

| Field | Required | Notes |
|---|---|---|
| Employee ID | Yes | Numeric recommended (e.g., 101, 102, 103) |
| First Name, Last Name | Yes | Full name for reports |
| Labor Rate (Standard) | Yes | Used for job cost calculations |
| Overtime Rate | Optional | If OT is tracked separately |
| Department | Optional | Useful for department-level reporting |
| Default Work Center | Optional | If operator primarily works one area |
| Active Status | Yes | Set to Active |
| PIN / Badge Number | Optional | For badge scanner clock-in |
| User Account Link | Optional | Links to JobBOSS user login if operator also uses desktop system |

**Step 4:** Save the record.

**Step 5:** Create a barcode ID badge or card for the employee. The barcode encodes the Employee ID in Code 128 or Code 39 format. This allows the operator to scan their badge rather than type their ID at the terminal.

### 5.4.2 Labor Rates and Job Costing Accuracy

The labor rate entered on each employee record is the rate used to calculate actual labor cost posted to jobs. This rate should represent the fully-burdened cost of that employee's labor, not just their hourly wage. A common approach:

| Rate Component | Example Value |
|---|---|
| Base hourly wage | $24.00/hr |
| Payroll taxes and benefits (burden %) | + 30% = $7.20/hr |
| Fully burdened rate | $31.20/hr |

If JobBOSS is configured to use a single standard labor rate for all employees rather than individual rates, ensure that rate is set at a level that accurately represents the average cost of labor on the floor. Under-setting the rate will make all jobs appear more profitable than they actually are.

### 5.4.3 Employee ID Discipline

> **POLICY REQUIREMENT:** Every operator must have and use their own unique Employee ID at all data collection terminals. Sharing IDs (e.g., a general "SHOP" login that multiple people use) destroys accountability and renders individual performance data meaningless. If a quality escape occurs, Die-Mension must be able to identify which specific operator performed a specific operation.
>
> Enforce this policy from day one. Make it clear in the employee handbook or during onboarding that shared IDs are not permitted. Supervisors must spot-check terminal logs regularly to identify shared-ID activity.

---

## 5.5 The Operator Workflow at a Data Collection Terminal

This section documents the complete step-by-step process an operator follows at a data collection terminal for the most common scenarios. Post a simplified version of this workflow at each terminal as a visual aid.

### 5.5.1 Starting a New Job Operation (Clocking In)

**Step 1: Identify at the Terminal**

Approach the terminal. The screen should display the idle/home state with an Employee ID entry prompt.

Either:
- Scan your employee badge barcode (recommended)
- Type your Employee ID and press Enter/OK

**Step 2: Select or Enter Job Number**

The terminal will display an active job list or a blank Job Number field.

Either:
- Scan the job number barcode from your job traveler (recommended)
- Type the job number (must be numeric) and press Enter/OK

**Step 3: Select the Operation**

The terminal will display the routing steps for the selected job. Select the operation you are beginning:
- Scan the operation barcode (if printed on the traveler)
- Select from the displayed list (e.g., "OP 20 — Mill Pocket")
- Type the operation sequence number

**Step 4: Confirm Work Center**

The terminal will display the work center associated with the operation. If you are performing the operation on a different machine than the default, update the work center.

**Step 5: Select Time Type**

Select either:
- `SETUP` — if you are setting up the machine, loading fixtures, programming, or performing first-article inspection before production begins
- `RUN` — if setup is complete and you are producing parts

**Step 6: Confirm Start**

Press `Start`, `Clock In`, or equivalent confirmation button. The system records the start timestamp.

The terminal will display a confirmation screen showing:
- Your name
- Job number
- Operation
- Work center
- Time started
- Whether you are in Setup or Run mode

Leave this screen active. Do not close or navigate away from it while the operation is in progress.

### 5.5.2 Completing a Job Operation (Clocking Out)

**Step 1:** Return to the terminal. If the session has timed out, re-identify yourself (Employee ID) and locate the active operation from the active operations list.

**Step 2:** Select the active operation you are completing.

**Step 3:** Enter quantity information:
- **Quantity Good:** Enter the number of pieces produced that meet specification
- **Quantity Scrapped:** Enter the number of pieces rejected, destroyed, or otherwise unacceptable
- **Quantity Moved:** Some setups use this to indicate pieces moved to the next operation or inspection

> **Note on Quantity Entry for Tool and Die Work:** In a job shop producing custom tooling, most operations produce exactly 1 piece (e.g., "Mill the punch blank"). In this case, Quantity Good = 1 unless the job calls for multiples. Remind operators that this field still must be filled in — leaving it blank or entering 0 will post the operation as complete with zero output, which creates a costing anomaly.

**Step 4:** If switching from Setup to Run within the same session, and your terminal configuration supports it, you may be able to transition in-place rather than clocking out and back in. Check with your JobBOSS administrator whether your terminal is configured for this.

**Step 5:** Select the completion status:
- `Complete` — this operation is finished; move to next operation
- `Partial Complete` — some quantity done, more work needed on this operation later
- `Clock Out Only` — clocking out for break or end of shift, operation not yet done

**Step 6:** Press `Complete` or `Clock Out`. The system records:
- End timestamp
- Elapsed time (end - start = labor hours)
- Labor cost (elapsed hours × employee labor rate)
- Quantities
- Posts all of the above to the job record

**Step 7:** The terminal returns to the idle/home screen, ready for the next operator or the next operation.

### 5.5.3 Clocking Out for Break or End of Shift Without Completing the Operation

This is a common scenario: an operator starts a machining operation, leaves for lunch or end of shift, and resumes the next day.

1. At the terminal, select the active operation.
2. Select `Pause`, `Clock Out — Operation in Progress`, or equivalent option (the label varies by version).
3. Enter any partial quantity completed so far (if applicable).
4. Confirm the clock-out.
5. The system records the time worked so far, posts a partial labor entry, and marks the operation as `In Progress` rather than `Complete`.
6. When the operator returns, they clock back in to the same operation, and the system begins accumulating time again from the new clock-in.

All time segments are summed into the total actual labor hours for that operation on the job.

### 5.5.4 Running Multiple Operations Simultaneously

An operator running a CNC machine unattended while setting up a manual machine represents one of the most common multi-machine scenarios in a job shop. JobBOSS supports clocking in to multiple operations simultaneously.

1. Clock in to the first operation as described in 5.5.1.
2. Without clocking out, approach the same or a different terminal.
3. Re-identify yourself.
4. The terminal will show your currently active operation(s) and also present the option to start a new operation.
5. Start the second operation (different job number or different operation on the same job).
6. Both operations are now accumulating time in parallel.

When one is complete, clock out of that specific operation only. The other continues until you clock out of it separately.

> **Management Note on Parallel Operations and Costing:** When an operator is clocked into two simultaneous operations, the labor time accrues to BOTH operations at the full rate. This can cause "double-counting" of labor in job costing. Some shops handle this by manually adjusting the labor allocation after the fact (splitting the hours 50/50 between the two jobs). JobBOSS does not automatically prorate parallel time — discuss this with your controller or cost accountant and establish a consistent policy before go-live.

### 5.5.5 Entering Material Issues at the Terminal

If your JobBOSS configuration allows material issuance from the data collection terminal:

1. During or after clocking out of an operation, select `Issue Material` or `Component Issue`.
2. Enter or scan the part number of the material being issued.
3. If the part is lot-controlled, the system will display available lots — select the correct lot (heat number).
4. Enter the quantity issued.
5. Confirm.

This completes the material traceability link at the shop floor level: operator, job, operation, material lot, all captured in a single session at the terminal.

---

## 5.6 Setup Time vs. Run Time

The distinction between setup time and run time is one of the most analytically valuable data points that data collection can capture — and one of the most commonly lost due to operator habit and training failures.

### 5.6.1 Definitions

**Setup Time** is the time an operator spends preparing everything required before the first good production piece can be made. Setup time includes:
- Reading the job traveler and understanding the operation requirements
- Retrieving tooling, fixtures, gages, and material from storage
- Loading the workpiece into the machine
- Loading the program (CNC) or configuring the machine (manual)
- Setting tool offsets, work offsets, datum points
- Running and measuring the first piece
- Making adjustments based on first-piece measurement
- Obtaining first-piece sign-off from inspection (if required)

Setup time does not scale with quantity. If it takes 2 hours to set up a CNC operation, it takes 2 hours whether you produce 1 piece or 100 pieces.

**Run Time** is the time spent producing parts after setup is complete. Run time includes:
- Load/unload cycle time
- Machine cycle time
- In-process inspection and measurement
- Minor adjustments to maintain tolerance during the run

Run time scales directly with quantity.

### 5.6.2 Why the Distinction Matters for Estimating and Costing

| Scenario | If All Time Entered as Run | If Setup and Run Separated |
|---|---|---|
| Job with 1 piece: 2 hrs setup, 0.5 hrs run | Run time/pc appears as 2.5 hrs | Setup = 2 hrs, Run = 0.5 hr/pc |
| Next quote for same 1-piece job | Estimate 2.5 hrs run — correct for 1 piece | Can separately estimate 2 hrs setup + 0.5 hr run |
| Quote for 10-piece run of same part | Estimate 25 hrs total — wildly over | Estimate 2 hrs setup + 5 hrs run = 7 hrs total — correct |
| Quote for 50-piece run | Estimate 125 hrs — impossibly over | 2 hrs setup + 25 hrs run = 27 hrs — correct |

The impact of misclassifying setup as run time compounds dramatically at higher quantities. A shop that cannot separate setup from run time cannot generate accurate quantity-variable quotes.

### 5.6.3 Common Reasons Operators Enter Everything as Run Time

- The terminal interface does not make it obvious which mode to select
- Operators do not understand the business reason for the distinction
- Supervisors do not correct the behavior early on
- The path of least resistance is to select "Run" because it is the default

### 5.6.4 Corrective Measures

1. **Laminated visual aid at every terminal:** Post a card that shows exactly how to select Setup vs. Run, with a simple example: "Setting up the machine = SETUP. Running parts after setup = RUN."

2. **Supervisor spot audit daily for the first 60 days:** Each morning, pull the prior day's labor entries and verify that setup operations are appearing as Setup. When an operator enters setup time as Run, correct it immediately and explain why.

3. **Estimating feedback loop:** Monthly, compare the estimated setup time on operations to the actual setup time reported. Share this data with operators. When they see that their data directly affects how future jobs are quoted, compliance improves.

4. **Training document:** Create a one-page "What is Setup vs. Run?" document with examples relevant to Die-Mension operations (e.g., "Setting up the Haas to run a punch blank = Setup. Running the first operation on the punch = Run.").

---

## 5.7 The Batch Entry Function

Batch entry is a data entry mechanism within JobBOSS that allows an operator or supervisor to enter labor hours for multiple operations retroactively, rather than clocking in and out in real time at the terminal.

### 5.7.1 How Batch Entry Works

1. Navigate to the data collection module and select `Batch Entry` (or equivalent).
2. Select the employee whose time is being entered.
3. For each operation worked:
   - Enter the job number
   - Enter the operation/routing step
   - Enter the work center
   - Enter the start time and end time (or start time and duration)
   - Enter setup/run classification
   - Enter quantities
4. Submit the batch. The system posts all entries simultaneously.

Batch entry produces the same records in the database as real-time terminal entry — the difference is that batch entries are entered after the fact rather than as work occurs.

### 5.7.2 Legitimate Uses for Batch Entry

| Scenario | Acceptable? | Notes |
|---|---|---|
| System downtime during shift — catch-up after restoration | Yes | Document the downtime cause |
| Power outage that prevented terminal use | Yes | Document with supervisor approval |
| New employee in first week — supervisor-supervised entry until trained | Yes | Should transition to real-time within 2 weeks |
| Operator forgot to clock in for a single operation | Yes (occasional) | Supervisor approval required |
| All time entered at end of shift as daily routine | No | Defeats real-time visibility |
| All time entered weekly by supervisor for entire team | No | Invalidates all WIP reporting |
| Convenience preference — "too far from terminal" | No | Add a terminal closer to the work area |

### 5.7.3 Why Using Batch as the Default Practice Is Harmful

When batch entry becomes the shop's standard practice instead of real-time terminal entry, the following business consequences occur:

**Loss of Real-Time WIP Visibility:** Management cannot answer the question "What is happening right now in the shop?" because all data is hours or days behind. Customer inquiries about job status cannot be answered from the system — someone must physically walk the floor.

**Scheduling Accuracy Collapse:** JobBOSS scheduling uses operation start/complete times to adjust the schedule dynamically. Batch-entered completions posted at 4:00 PM for work done between 7:00 AM and 3:00 PM mean the schedule was running on stale data for nine hours. Dispatching decisions made during the day were based on false information.

**Bottleneck Blindness:** Real-time data collection enables management to see when a work center's queue is building faster than it is being consumed. Batch entry makes this invisible until the end of the day — by which time the bottleneck has already caused delays.

**Accuracy Degradation:** Memory is imperfect. When an operator enters time for eight operations at the end of a shift, the hours they recall for each operation will be estimates — not measured values. The data that populates the job cost reports will be fictional.

**Accountability Elimination:** If a supervisor enters all time for all employees from memory at the end of the week, there is no way to verify the accuracy of any individual time entry or to identify operators who are not meeting expected productivity standards.

### 5.7.4 Recommended Policy for Die-Mension

Establish and document a formal data collection policy:

> **Die-Mension Data Collection Policy:**
>
> 1. All shop floor operators are required to use data collection terminals in real time. This means clocking in at the start of each operation and clocking out upon completion or departure from the operation.
>
> 2. Batch entry is permitted only in cases of system unavailability (downtime, power outage, network failure) and requires supervisor approval before posting.
>
> 3. End-of-shift batch entry to catch up unreported time is not permitted as a routine practice. Operators who consistently fail to use terminals in real time will receive corrective coaching.
>
> 4. Supervisors will review the prior day's labor entries every morning and address any gaps before the morning production meeting.
>
> 5. Compliance with real-time data entry is a performance expectation for all shop floor personnel.

Post this policy at each terminal location. Include it in onboarding documentation.

---

## 5.8 Data Flow from Collection to Job Costing

Understanding the data flow from the moment an operator clocks in to the moment a cost appears on a job is essential for diagnosing discrepancies and explaining the system to operators and management.

### 5.8.1 Step-by-Step Data Flow

```
[Operator Clocks In at Terminal]
         |
         v
[JobBOSS Creates a "Labor Transaction — Open" record]
  - Employee ID
  - Job Number
  - Operation
  - Work Center
  - Start Timestamp
  - Time Type (Setup/Run)
         |
         v
[Operator Clocks Out / Completes Operation]
         |
         v
[JobBOSS Closes the Labor Transaction]
  - Adds End Timestamp
  - Calculates Duration: End - Start = Elapsed Hours
  - Calculates Labor Cost: Elapsed Hours × Employee Labor Rate
  - Records Quantity Good and Quantity Scrapped
         |
         v
[Labor Cost Posted to Job Record]
  - Job Actual Labor Cost increases
  - Operation marked as Complete (or Partial)
  - WIP value of job updated
         |
         v
[Job Cost Report Reflects Actual Labor]
  - Actual Hours vs. Estimated Hours: Variance visible
  - Actual Labor Cost vs. Estimated Labor Cost: Variance visible
  - Actual Labor Rate vs. Standard Rate: Efficiency calculation possible
```

### 5.8.2 Timing of Cost Posting

In standard JobBOSS configurations, labor costs post to the job immediately upon clock-out confirmation. There is no batch processing delay — the cost is on the job within seconds of the operator completing the clock-out. This means:

- A job cost report run at 10:15 AM will include all labor clocked out before 10:15 AM
- A job cost report run at 10:16 AM will include any additional clock-outs in that one-minute window
- Management has near-real-time job cost visibility throughout the day

### 5.8.3 What Happens if an Operator Clocks In But Never Clocks Out

If an operator clocks in to an operation and then walks away without clocking out (common causes: forgot, shift ended, left for the day), the following occurs:

- The labor transaction remains open indefinitely
- The job shows the operation as "In Progress"
- No cost is posted until the transaction is closed
- The operator's employee record may show them as still active on that operation

Over time, accumulated open (unclosed) transactions skew job status reports and make WIP reporting unreliable.

**Resolution:** Supervisors should run a "Missing Clock-Out" or "Open Labor Transactions" report daily. For each open transaction, the supervisor either:
- Confirms with the operator and closes it with the correct end time
- Makes a manual adjustment to close it with an estimated end time and documents the reason

### 5.8.4 Estimated vs. Actual Variance: Using the Data

Once real-time data collection is producing reliable data (typically 4–8 weeks after go-live), the actual vs. estimated labor variance report becomes one of the most powerful management tools available:

| What the Variance Tells You | Action to Take |
|---|---|
| Actual significantly higher than estimated on most jobs | Estimates are too optimistic — review estimating standards |
| Actual significantly lower than estimated on most jobs | Estimates are too conservative — adjust for competitive quoting |
| High variance on CNC operations specifically | Review CNC cycle time estimates; check program efficiency |
| High variance on specific work center | Investigate machine condition, tooling, or operator skill issue |
| High variance on one specific operator's jobs | Investigate productivity or training need |
| Variance increasing over time | Check for data entry quality degradation — increase supervisor reviews |

---

## 5.9 Barcode Scanning Strategy for Die-Mension

A well-designed barcode scanning strategy dramatically reduces terminal data entry time and errors. An operator who can scan their badge, scan the job traveler, and scan the operation code completes the entire clock-in process in under 10 seconds — compared to 30–90 seconds of manual typing.

### 5.9.1 What to Barcode

| Item | Barcode Encodes | Label Location | Update Frequency |
|---|---|---|---|
| Employee ID Badge | Employee ID number | Badge or laminated card | Once per employee |
| Job Traveler | Job Number | Header of traveler | Once per job |
| Operation Cards | Operation Sequence Number | Top of each operation section on traveler, or separate operation card | Once per job |
| Bin Locations | Bin Code | Rack label, wall label | Once per bin |
| Part Numbers | Part Number | Part storage label, bin tag | Once per part |
| Work Center IDs | Work Center Code | Each machine/station | Once per work center |

### 5.9.2 Barcode Format Selection

**Code 128:** Recommended as the primary format for Die-Mension.
- Handles alphanumeric and numeric data
- Compact and high-density — fits more data in less space
- Readable by all barcode scanners, including inexpensive models
- Supported natively by JobBOSS label printing functions

**Code 39:** Acceptable alternative. Slightly less dense but widely compatible.
- Easier to print at lower DPI
- Good for labels produced on basic laser printers

**QR Code (2D):** Use for supplemental data when more information must be encoded.
- Useful for encoding URLs to job-specific instructions or drawings stored on a server
- Requires a 2D scanner (standard USB keyboard-wedge scanners typically read 2D codes if they are 2D-capable models)
- Not recommended as the primary scanning format for job numbers due to size and scan-angle sensitivity

**Recommendation:** Standardize on Code 128 for all job number and employee ID barcodes. Use QR only for secondary information links.

### 5.9.3 Label Printing

**From within JobBOSS:** The system includes built-in label printing for job travelers that includes job number barcodes. Enable this and ensure the job traveler print format includes a scannable barcode in the header.

**Dedicated label printer:** For high-volume label production (bin labels, employee badges), a thermal transfer label printer (e.g., Zebra ZD420, Zebra ZT410) produces durable, long-lasting labels:
- Thermal transfer labels resist shop fluids, oils, and moderate temperature exposure
- Cost approximately $0.03–$0.10 per label
- Can be connected to the JobBOSS workstation via USB or network

**Basic laser printer labels:** Acceptable for job traveler barcodes that will not be exposed to coolant or heavy handling. Use standard 8.5" × 11" label stock (Avery or equivalent). Print at minimum 300 DPI; 600 DPI preferred for Code 128.

**Label Durability in Machine Shop Environments:**
- Standard paper labels degrade within minutes if exposed to coolant or cutting fluids
- For labels in wet or oily areas (near grinders, mills), use:
  - Self-laminating label stock
  - Thermal transfer labels in polyester or vinyl substrate
  - Laminating pouches for critical labels (employee badges, permanent bin labels)

### 5.9.4 Scanner Placement and Ergonomics

- Mount scanners on a fixed cable-tethered holder next to the terminal screen — do not rely on operators to locate the scanner before each use
- Position the scanner holder at a natural arm height for the average operator — approximately 42–48 inches from the floor
- Use a coiled retractable cable so the scanner returns to its holder after use
- Test scanner read distance for the barcode sizes you are printing — most inexpensive scanners read Code 128 reliably at 2–10 inches from the label

---

## 5.10 Monitoring and Accountability

Implementing data collection without a monitoring and accountability process is like installing a security camera and never watching the footage. The system only delivers its value if management actively uses the data it generates.

### 5.10.1 Daily Supervisor Review Process

Establish a non-negotiable daily routine:

**Every morning, before the production meeting:**

1. **Pull the Labor Entry Report for the prior day.** Review by employee. Identify:
   - Any employee with zero hours — were they absent? Did they fail to clock in?
   - Any employee with suspiciously low hours — less than expected for their shift
   - Any employee with unusually high hours — possible clock-in-but-no-clock-out error

2. **Pull the Open Operations Report.** This shows all operations where an operator clocked in but has not yet clocked out. For each open transaction:
   - If the operation is genuinely in progress today, no action needed
   - If the operation should have been completed yesterday, follow up with the operator and close it correctly

3. **Pull the Job Status Report.** Review jobs that are expected to be in progress. Verify their status reflects the actual shop floor state. If a job shows zero activity for 24 hours but is supposed to be in production, investigate.

4. **Address discrepancies immediately.** Corrections become less accurate and less legitimate over time. A correction made same-day is credible. A correction made one week later is an estimate.

### 5.10.2 Management Reports for Data Collection Compliance

| Report Name | What It Shows | How to Access | Review Frequency |
|---|---|---|---|
| Labor by Employee | Hours entered per employee per day/week | Reports → Labor → By Employee | Daily |
| Open Labor Transactions | Operations with no clock-out | Reports → Data Collection → Open Transactions | Daily |
| Actual vs. Estimated Labor | Per-job variance between estimate and actual | Reports → Job Cost → Actual vs. Estimated | Weekly per job |
| Job Status Report | Current operation status for all active jobs | Reports → Job Management → Job Status | Daily |
| Work Center Load Report | Operations in queue and in progress per work center | Reports → Scheduling → Work Center Load | Daily |
| Labor Efficiency by Employee | Actual hours vs. standard hours by operator | Reports → Labor → Efficiency (if available) | Weekly |
| Missing Clock-Out Report | Employees who clocked in with no subsequent clock-out | Reports → Data Collection → Clock-In Audit | Daily |

### 5.10.3 Connecting Data Quality to Performance Culture

Data collection compliance is a professional expectation, not a optional courtesy. Make this explicit:

1. Include data collection compliance as a performance expectation in all shop floor position descriptions.
2. Review compliance metrics with each operator in their quarterly or annual performance review.
3. When patterns of non-compliance appear (consistent late entries, missing entries, incorrect time type selection), address them in the moment with coaching — not months later in a formal review.
4. Celebrate accuracy wins. When the job cost report shows that actual matches estimated within 5% on a complex tooling job, share that result with the team and explain that it happened because data was entered accurately in real time.

---

## 5.11 Common Data Collection Mistakes and Fixes

The following table consolidates the most frequently encountered data collection failures at job shops implementing JobBOSS for the first time, along with specific corrective actions. Use this table as a go-live checklist and a troubleshooting reference.

| Mistake | Symptom in JobBOSS | Specific Fix |
|---|---|---|
| Collection Terminal records not created before go-live | Terminals show errors or do not load the data collection screen | Create terminal records in Table Maintenance → Collection Terminals before any operator attempts to use the system |
| Alpha characters in job numbers | Terminal rejects job number entry; operators frustrated and stop using terminals | Switch to numeric-only auto-generated job numbers immediately; consult administrator for migration plan if alpha numbers are already in use |
| All time entered via Batch at end of shift as routine | Job status shows all jobs as "Not Started" or "In Progress" all day; WIP report is meaningless | Enforce real-time entry policy; require supervisor approval for batch entries; post compliance data in morning meetings |
| Setup time classified as Run time | Job cost shows high run rate per piece; quantity-variable quotes are inaccurate; setup amortization is invisible | Post setup/run decision guide at each terminal; daily supervisor review for first 60 days; estimating variance review monthly |
| Operators sharing Employee IDs | Cannot distinguish individual performance; accountability reports are meaningless | Issue individual badges; enforce one-ID policy; disable shared ID accounts if any exist |
| Terminals placed too far from work areas | Operators skip entries because terminal is inconvenient | Add additional terminals; survey operators for desired locations; aim for terminals within 30 feet of each primary work area |
| No operator training before go-live | Operators unsure how to use terminals; data entry is slow and error-prone; frustration leads to abandonment | Conduct a hands-on training session before go-live; create a one-page laminated quick-reference card for each terminal |
| DCMobile used for job attachment viewing | App crashes; operators lose trust in the system | Restrict DCMobile use to time entry only; provide fixed-PC terminal for document viewing |
| Missing clock-out entries accumulate | Job status shows operations as perpetually "In Progress"; labor costs are delayed; WIP balance is inflated | Run open-transaction report daily; close unclosed entries same-day; train operators on proper clock-out procedure |
| Material not issued through data collection | Job cost shows zero material cost; lot traceability is not captured at job level | Require material issue to be performed at terminal at time of material withdrawal; train material handlers on lot selection at issue |
| No accountability process after go-live | Compliance degrades within 4–8 weeks; data quality returns to pre-implementation levels | Implement daily supervisor review as a standing process; include data entry compliance in weekly production meeting agenda |
| Incorrect quantity good/scrapped entries | Job cost scrap rate is wrong; customer-required scrap reporting is inaccurate | Train operators that scrap includes pieces that are not salvageable; provide examples of what constitutes a "good" piece vs. a "scrapped" piece specific to Die-Mension operations |
| Work center not selected correctly at terminal | Labor hours post to wrong work center; work center capacity reports are distorted | Set default work centers on each terminal record; train operators to verify work center before confirming clock-in |
| Labor rate not set on employee records | Labor cost shows as $0 on jobs; job profitability appears artificially high | Verify every active employee record has a non-zero labor rate before go-live; include labor rate review in annual employee record audit |
| Job numbers not barcoded on travelers | Operators type job numbers manually; slower entry, higher error rate | Ensure job traveler print format includes Code 128 barcode of job number; verify all active jobs have travelers printed with barcodes |

---

## 5.12 Advanced Configurations and Integration Points

### 5.12.1 Integrating Data Collection with Scheduling

When data collection is functioning reliably (4–8 weeks post-go-live), connect it explicitly to the scheduling process:

- **Operation completions** reported through data collection automatically advance the job's status in the scheduling module
- **Actual start times** reported through data collection can replace scheduled start times, giving the schedule a real-time grounding point
- **Work center utilization** data from labor entries feeds the work center load report, which is the primary tool for identifying scheduling bottlenecks

For Die-Mension's scheduling workflow, review actual operation durations weekly and compare them to the estimated durations used in the routing. Where actual routinely exceeds estimated by more than 20%, update the routing standards. This creates a continuous improvement loop: data collection → actual hours → routing update → better estimates → better scheduling.

### 5.12.2 First Article Inspection Integration

For Die-Mension, first-article inspection (FAI) is a critical step on most jobs. Data collection can be configured to require first-article sign-off before production quantity can be clocked in:

1. Create a distinct operation on the routing for "First Article Inspection" or "FAI"
2. Assign this operation to the QC work center
3. Configure the subsequent Run operations to require the FAI operation to be marked complete before Run time can be posted
4. This ensures: (a) FAI time is captured separately, (b) operators cannot bypass the inspection step without the system knowing, and (c) job costing includes the true cost of first-article inspection

### 5.12.3 Downtime and Non-Productive Time Tracking

Data collection can capture not only productive time but also downtime categories. This is optional but valuable for tracking machine reliability and indirect labor efficiency.

Common downtime codes for a tool and die shop:

| Code | Description | Example |
|---|---|---|
| DT-MAINT | Unplanned maintenance / machine down | Spindle bearing failure on Haas |
| DT-TOOLING | Waiting for tooling | EDM wire spool empty, waiting for replacement |
| DT-MATERIAL | Waiting for material | Steel not yet delivered to machine |
| DT-PROGRAM | Waiting for or debugging CNC program | Program not yet posted from CAM |
| DT-DRAWING | Waiting for drawing or work instruction | Traveler not yet available |
| DT-BREAK | Paid break (if tracked) | Morning break, lunch |
| DT-MEETING | Production or safety meeting | Morning production meeting |
| DT-TRAINING | Training time | New operator training on surface grinder |

To enable: create a work order or indirect charge code for each downtime category. Train operators to clock in to a downtime code when they are unable to produce. This data feeds utilization analysis and identifies systemic bottlenecks (e.g., if DT-DRAWING accounts for 8% of operator time across all work centers, the drawing release process is a constraint).

### 5.12.4 Shift and Overtime Configuration

If Die-Mension operates multiple shifts or regularly incurs overtime:

1. Define shift records in JobBOSS Table Maintenance: `Table Maintenance` → `Shifts`
2. Assign each shift a start time, end time, and break schedule
3. Assign employees to shifts on their employee records
4. Configure the system to apply overtime rates automatically when hours exceed defined thresholds (8 hours per day, 40 hours per week — per Ohio state labor law)
5. Verify that data collection terminal records have the correct shift assigned for auto-clock-out at shift end (optional — some shops prefer operators always manually clock out)

### 5.12.5 External Data Collection Integration

For shops considering future growth or more sophisticated data capture, JobBOSS supports integration with:

- **Machine monitoring systems** (e.g., Caron Engineering, Scytec DataXchange): These systems connect directly to machine CNC controllers and automatically report machine cycle times, spindle utilization, and alarm conditions — without operator input. Machine monitoring data can be imported alongside JobBOSS data collection records to provide a complete picture of labor time vs. machine cycle time.

- **MES (Manufacturing Execution Systems):** For Die-Mension at its current scale, a standalone MES is likely unnecessary. JobBOSS data collection provides MES-level functionality for job shops. Revisit this if volume grows to 5,000+ active operations per month.

- **ERP integrations:** If a customer requires Die-Mension to report production status into their supplier portal or ERP system, the JobBOSS data collection records provide the source data for those reports.

---

*End of Section 4 and Section 5.*

*These sections are part of the Die-Mension JobBOSS/JobBOSS² Master Reference Manual. For questions regarding configuration, contact your JobBOSS administrator or refer to the JobBOSS Help Center at help.jobboss.com.*
```

---

Here is a summary of what was produced:

**Section 4: Lot and Serial Number Tracking Deep Dive** covers the full business rationale for heat number traceability (automotive, aerospace, ISO/AS9100D requirements), a detailed comparison table of lot vs. serial control, a step-by-step procedure for enabling lot control with explicit warnings about the critical timing requirement, the complete five-phase heat number tracking workflow from PO creation through customer inquiry response, the multiple-heats-per-shipment problem with three options and a recommended workaround, all four primary lot tracking reports with access instructions and use cases, serial control configuration and use, and coil/bar stock drop tracking with physical label conventions and UOM guidance.

**Section 5: Shop Floor Data Collection Deep Dive** covers the business value case for data collection, a full hardware options comparison table including confirmed community-validated devices and documented DCMobile app issues, step-by-step terminal configuration with the critical numeric job number warning, employee setup and labor rate configuration, the complete operator workflow for clocking in, clocking out, pausing, and running parallel operations, a detailed setup vs. run time analysis with a quantified estimating impact example, the batch entry function with a policy template and explicit harm analysis, the complete data flow from terminal clock-in to job cost posting, a barcode scanning strategy with format recommendations and label durability guidance, daily accountability processes with a full management report table, a comprehensive 14-row mistake/fix table, and advanced topics including scheduling integration, first article inspection workflow, downtime codes, and shift configuration.


---

No applicable skill found. Proceeding to write the two sections directly using all provided research data.

---

```markdown
# JobBOSS / JobBOSS² Master Reference Manual
## Die-Mension Tool & Die — Brunswick, OH

---

# SECTION 6: DATABASE AND TECHNICAL ARCHITECTURE

---

## 6.1 Database Platform Overview

### 6.1.1 Backend Database Engine

JobBOSS uses **Microsoft SQL Server** as its backend database engine. This replaced an earlier Microsoft Access backend in older versions of the software. All production data — jobs, customers, materials, routing operations, notes, and financials — resides in a single SQL Server database that can be accessed by authorized tools and reporting systems.

**Confirmed SQL Server Compatibility:**

| SQL Server Version | Edition | Architecture | Compatibility with JobBOSS 12.x |
|---|---|---|---|
| SQL Server 2017 | Enterprise | 64-bit | Confirmed |
| SQL Server 2017 | Standard | 64-bit | Confirmed |
| SQL Server 2017 | Express | 64-bit | Confirmed |

> **Note for Die-Mension:** Confirm which edition is installed on the server by running `SELECT @@VERSION` in SQL Server Management Studio (SSMS). Express edition has a 10 GB database size limit and does not support SQL Server Agent for scheduled jobs.

### 6.1.2 Database Names

The JobBOSS database may be named differently depending on installation history and configuration:

| Common Database Name | Context |
|---|---|
| `JobBoss32` | Default name used in most installations; legacy name carried forward from early versions |
| `production` | Alternate name seen in some installations |

To confirm the correct database name for Die-Mension's installation:

1. Open **SQL Server Management Studio (SSMS)**
2. Connect to the SQL Server instance used by JobBOSS
3. Expand **Databases** in the Object Explorer
4. The JobBOSS database will typically be named `JobBoss32` or `production`
5. Verify by expanding **Tables** and confirming the presence of `Jobs`, `Customers`, and `Job_Operations`

### 6.1.3 Architecture Summary

```
[JobBOSS Client Application]
        |
        | (Named Pipes / TCP-IP)
        v
[SQL Server Instance]
        |
        +-- Database: JobBoss32 (or "production")
              |
              +-- Tables (Jobs, Job_Operations, Materials, etc.)
              +-- Views
              +-- Stored Procedures
```

External tools such as Crystal Reports, Excel/VBA, Power BI, and custom applications connect to the same SQL Server database via ODBC, OLE DB, or direct ADO.NET connections.

---

## 6.2 Finding the Data Dictionary and Schema Utility

### 6.2.1 JobBOSS Data Dictionary

The official JobBOSS Data Dictionary documents every table, field, data type, and relationship in the database schema.

- **Document Number:** 27.240.736
- **Applies To:** JobBOSS version 12.0
- **Location:** ECI/Exact Software customer portal

**Steps to Access the Data Dictionary:**

1. Navigate to **https://customers.jobboss.com**
2. Log in with your Die-Mension customer account credentials
3. Click on **"Release and Download Info"**
4. Select **"Knowledge Base"**
5. Navigate to **"Utility and Other"** → **"Utilities"**
6. Search for document number **27.240.736** or browse for the Data Dictionary entry
7. Download the PDF document for offline reference

> **Recommendation:** Download and store a copy of document 27.240.736 in the Die-Mension shared drive under `/Documentation/JobBOSS/`. This document is indispensable for building custom reports and integrations.

### 6.2.2 JobBOSS Database Schema Utility

The **JobBOSS Database Schema Utility**, published by Exact Software, provides a visual, interactive view of the database structure including table relationships, foreign keys, and field-level details.

- **Purpose:** Visual visibility into database structure without requiring SSMS or raw SQL queries
- **Publisher:** Exact Software
- **Access:** Available on the customer portal alongside other utilities

This utility is particularly useful when:
- Building new Crystal Reports
- Writing SQL queries against unfamiliar tables
- Diagnosing join problems between tables
- Onboarding new staff or AI tools to the database layout

---

## 6.3 Key Tables Reference

The following tables are confirmed to exist in the JobBOSS database. This list covers the tables most relevant to job management, routing, materials, customers, and notes.

| Table Name | Description |
|---|---|
| `Jobs` | Main job header table; contains one record per job with job number, status, customer, due date, etc. |
| `Job_Operations` | Job routing operations; contains one record per operation per job, including work center, times, costs, and status |
| `Materials` | Material and part library; master list of parts/materials used in jobs |
| `Bill_Of_Jobs` | Component job tree structure; supports multi-level job hierarchies (parent/child job relationships) |
| `Address` | Address records for customers, vendors, and contacts; has a known double-key design issue (see Section 6.10) |
| `NoteHeaders` | Maps note tokens to specific database tables; the lookup/index layer of the notes system |
| `SpecificNotes` | Stores the actual note text content and note title/description |
| `ObjectNotes` | Associates notes to specific records; the linking layer between `NoteHeaders`, `SpecificNotes`, and the parent record |
| `jobroute` | Job routing and work instructions; related to operation routing data |
| `Customers` | Customer master records; company name, contact info, credit terms, etc. |
| `Vendors` | Vendor master records; outside processor and supplier information |

> **For AI Assistants:** When helping with queries or reports, always confirm table names against this list and the Data Dictionary. Custom fields added by Die-Mension may appear in extended or custom tables not listed here.

---

## 6.4 Table Relationship Guide

### 6.4.1 Core Job Hierarchy

The primary relationships in JobBOSS revolve around the `Jobs` table as the parent record:

```
Customers
    |
    | (Jobs.Customer = Customers.Customer)
    v
Jobs  <-- Main job header
    |
    | (Job_Operations.Job = Jobs.Job)
    v
Job_Operations  <-- Routing steps / operations
    |
    | (Job_Operations.Material1 = Materials.Part_Number)
    v
Materials  <-- Material/part library
```

### 6.4.2 Bill of Jobs (Multi-Level Job Structure)

The `Bill_Of_Jobs` table supports hierarchical job structures where a top-level job has one or more component jobs:

```
Jobs (Top-Level)
    |
    +-- Bill_Of_Jobs.Root_Job_OID = Jobs GUID (top level)
    +-- Bill_Of_Jobs.Parent_Job_OID = parent job GUID
    +-- Bill_Of_Jobs.Component_Job_OID = child job GUID
    |
    v
Jobs (Component/Child)
```

**Bill_Of_Jobs Table Fields:**

| Field | Type | Description |
|---|---|---|
| `ObjectID` | GUID | GUID for this Bill of Job record |
| `Job_Operation_OID` | GUID | Purpose varies by context |
| `Root_Job_OID` | GUID | GUID of the Top Level Job |
| `Parent_Job_OID` | GUID | GUID of the Parent Job |
| `Component_Job_OID` | GUID | GUID of this component job |
| `Parent_Job` | varchar | Parent Job ID (human-readable) |
| `Component_Job` | varchar | This job's name (human-readable) |
| `Top_Lvl_Job` | varchar | Top level job name — **use this field for consistent results** |

> **Important:** When writing queries involving multi-level job structures, use `Top_Lvl_Job` (varchar) rather than GUID fields for more consistent and predictable results.

### 6.4.3 Notes System Table Relationships

The JobBOSS notes system uses three tables working together:

```
NoteHeaders
    |  (maps token to a specific table name)
    |
ObjectNotes
    |  (RefRowPointer links to the parent record)
    |  (NoteHeaderToken links to NoteHeaders)
    |  (SpecificNoteToken links to SpecificNotes)
    |
SpecificNotes
       (contains NoteContent = actual text)
       (contains NoteDesc = title/label)
```

**Reading notes for a job operation:**
1. Identify the `RowPointer` of the `Job_Operations` record
2. In `ObjectNotes`, find records where `RefRowPointer` matches that `RowPointer`
3. Join `ObjectNotes.SpecificNoteToken` to `SpecificNotes.SpecificNoteToken`
4. Read `SpecificNotes.NoteContent` for the note text

### 6.4.4 Job_Operations Key Fields for Joins

| Field | Join Target | Notes |
|---|---|---|
| `Job` | `Jobs.Job` | Primary join key to the Jobs table |
| `Material1` | `Materials.Part_Number` | Joins to the material library |
| `Part_Number` | `Materials.Part_Number` | Alternate part identifier |
| `Work_Center` | Work center table | References the work center used |
| `Vendor` | `Vendors.Vendor` | For outside operations |
| `WC_Vendor` | `Vendors.Vendor` | Work center vendor |

---

## 6.5 Crystal Reports Integration

### 6.5.1 How JobBOSS Uses Crystal Reports

JobBOSS delivers its standard reports through Crystal Reports. Custom user-defined reports are supported through a specific file-based mechanism:

- JobBOSS uses an `.htm` file containing **VBScript and SQL** to create a **temporary data file**
- Crystal Reports then uses that temporary file as its data source
- This architecture means reports are triggered through the JobBOSS interface but rendered by Crystal Reports

### 6.5.2 Step-by-Step Workflow for Creating a Custom Crystal Report

**Prerequisites:**
- Crystal Reports software installed on the workstation
- Access to the JobBOSS `Reporting` and `Reporting\User-Defined` directories
- Credentials to connect Crystal Reports to the SQL Server database
- Understanding of the JobBOSS Data Dictionary (document 27.240.736)

**Step 1: Plan the Data Requirements**

Before opening Crystal Reports, map out:
- Which tables will be queried
- Which fields are needed
- What filtering criteria apply (job status, date range, customer, etc.)
- Write a complete SQL SELECT statement that returns all required data in a single query

**Step 2: Create the Crystal Report File (.rpt)**

1. Open Crystal Reports
2. Choose **"Blank Report"**
3. When prompted for a data source, choose **"OLE DB (ADO)"**
4. Select **"Microsoft OLE DB Provider for SQL Server"**
5. Enter the SQL Server connection details (see Section 6.6 for connection strings)
6. Select the `JobBoss32` (or `production`) database

**Step 3: Use a Single SQL Command Object**

This is the most critical performance decision:

1. In the **Database Expert**, choose **"Add Command"** instead of selecting individual tables
2. Enter your complete SQL query as a single `SELECT` statement with all necessary `JOIN` and `WHERE` clauses
3. Do **not** use the Crystal Reports "Select Expert" or multiple Command objects for filtering

> **Best Practice:** Write a command that pulls all the data for the report in a single query. Make sure that you are using the `WHERE` clause in the command to filter your data instead of using the Select Expert.

**Step 4: Design the Report Layout**

1. Drag fields from the Field Explorer onto the report sections (Header, Details, Footer, Groups)
2. Set up Group By sections as needed
3. Add summary formulas in Group Footer or Report Footer sections
4. Format fields (currency, date, numeric)

**Step 5: Set Document Properties**

1. In Crystal Reports, go to **File → Document Properties**
2. Set the **Title** field to the exact name you want the report to appear under in JobBOSS
3. This title must **exactly match** the Report Name in JobBOSS — character-for-character, including capitalization and spacing

**Step 6: Save the .htm Trigger File**

1. Create or copy the `UD_Report.htm` file
2. This file must contain the VBScript and SQL that generates the temporary data file
3. Place the identical `UD_Report.htm` file in **both** of these directories:
   - `[JobBOSS Install Path]\Reporting\`
   - `[JobBOSS Install Path]\Reporting\User-Defined\`

> **Critical:** The `UD_Report.htm` file must be identical in both the `Reporting` and `Reporting\User-Defined` directories. If they differ, the report may fail or produce incorrect results.

**Step 7: Register the Report in JobBOSS**

1. In JobBOSS, navigate to **System → Reports → User-Defined Reports**
2. Add a new entry with the Report Name matching the Crystal Reports Document Properties Title exactly
3. Point to the `.rpt` file location
4. Test the report by running it from the JobBOSS interface

### 6.5.3 Crystal Reports Performance Best Practices

**The Single Command Rule**

The most common performance problem with JobBOSS Crystal Reports is using multiple Command objects or table selections instead of a single SQL command.

**Why Multiple Commands Are Slow:**

When multiple Command objects are used, Crystal Reports pulls all data from each command entirely into memory and then performs the joins in memory on the workstation. For large tables like `Job_Operations` or `Jobs`, this means thousands of rows are transferred over the network before any filtering occurs.

**Confirmed Performance Example:**

A custom Crystal Report was taking **4 minutes** to run. The fix: consolidate multiple Command objects into a single SQL query with proper `WHERE` and `JOIN` clauses. After the fix, the database server handled all filtering and joining — only the final result set was transferred to the workstation.

**Performance Checklist:**

| Rule | Correct Approach | What to Avoid |
|---|---|---|
| Data retrieval | Single SQL Command with WHERE clause | Multiple Command objects |
| Filtering | SQL WHERE clause in the Command | Crystal Reports Select Expert |
| Joining | SQL JOIN in the Command | Crystal Reports table linking |
| Aggregation | SQL GROUP BY / SUM in Command where possible | Crystal Reports running totals on large datasets |
| Indexes | Ensure SQL Server indexes exist on join keys | Full table scans on large tables |

---

## 6.6 ODBC Connection Setup

### 6.6.1 Overview

The JobBOSS SQL Server database is accessible via **ODBC** for external reporting tools including Excel, Crystal Reports, Power BI Desktop, and custom applications. Both Windows Authentication and SQL Server Authentication are supported.

### 6.6.2 Step-by-Step ODBC DSN Setup (Windows)

1. Open **Start → Windows Administrative Tools → ODBC Data Sources (64-bit)** (or 32-bit if connecting from a 32-bit application)
2. Click the **System DSN** tab (for machine-wide access) or **User DSN** (for current user only)
3. Click **Add**
4. Select **"SQL Server"** or **"ODBC Driver 17 for SQL Server"** from the list
5. Click **Finish**
6. Enter a **Name** for the DSN (e.g., `JobBOSS_Production`)
7. Enter an optional **Description**
8. In the **Server** field, enter the server name and instance:
   - Format: `SERVERNAME\INSTANCENAME`
   - Example with IP: `192.168.1.100\SQL Server`
9. Choose authentication method (see 6.6.3)
10. Select the database: `JobBoss32` or `production`
11. Click **Test Data Source** to verify connectivity
12. Click **OK** to save

### 6.6.3 Connection String Variants

**Windows Authentication (Recommended):**

```
Provider=sqloledb; Data Source=SERVERNAME\INSTANCENAME; Initial Catalog=DATABASENAME; Integrated Security=SSPI;
```

**SQL Server Authentication:**

```
Provider=sqloledb; Data Source=SERVERNAME\INSTANCENAME; Initial Catalog=DATABASENAME; User ID=username; Password=password;
```

**Legacy ODBC Format (documented from public sources):**

```
uid=S;pwd=;server=exserv;driver={ODBC 3.51 Driver};Database=JobBoss32;dsn=;
```

**OLE DB with SQL Authentication (documented from public sources):**

```
Provider=SQLOLEDB;Data Source=Exserv;Initial Catalog=production;User ID=support;Pwd=lonestar;
```

> **Security Note for Die-Mension:** The legacy connection strings above are documented for reference. Do not use default or shared credentials in production. Create a dedicated read-only SQL Server login for reporting connections. Use Windows Authentication wherever possible to avoid storing passwords in connection strings or report files.

### 6.6.4 Connection String Parameters Reference

| Parameter | Description | Example |
|---|---|---|
| `Provider` | OLE DB provider name | `sqloledb` |
| `Data Source` | Server name and instance | `SERVERNAME\INSTANCENAME` |
| `Initial Catalog` | Database name | `JobBoss32` |
| `Integrated Security=SSPI` | Use Windows Authentication | Omit `User ID` and `Password` |
| `User ID` | SQL login username | `reporting_user` |
| `Password` | SQL login password | (stored securely) |

---

## 6.7 VBA/Excel Direct Database Access

Excel with VBA can connect directly to the JobBOSS SQL Server database using ADO (ActiveX Data Objects). This enables live data pulls into Excel workbooks, custom dashboards, and automated reports without Crystal Reports.

### 6.7.1 Prerequisites

1. Microsoft Excel with VBA enabled
2. **Microsoft ActiveX Data Objects** reference enabled in the VBA IDE:
   - Open Excel → `Alt+F11` → Tools → References
   - Check **"Microsoft ActiveX Data Objects 6.1 Library"** (or latest available)
3. Network access to the SQL Server from the workstation
4. Valid credentials (Windows Auth or SQL Auth)

### 6.7.2 VBA Connection and Recordset Example

The following VBA code demonstrates opening a connection, reading data, adding records, and executing updates:

```vba
Dim cn As New ADODB.Connection
cn.Provider = "sqloledb"
cn.ConnectionString = "Data Source=SERVERNAME;Initial Catalog=JOBBOSS_DB;Integrated Security=SSPI"
cn.Open

Dim rs As New ADODB.Recordset
rs.Open "TableName", cn, adOpenDynamic, adLockOptimistic
rs.AddNew
rs.Fields("FieldName").Value = Range("A1").Value
rs.Update

cn.Execute "UPDATE table_name SET [Field_name] = 'some new value' WHERE [some other field] = 'criteria'"
```

**Replace the following placeholders:**

| Placeholder | Replace With | Example |
|---|---|---|
| `SERVERNAME` | SQL Server hostname or IP | `JOBBOSS-SERVER` or `192.168.1.100` |
| `JOBBOSS_DB` | Database name | `JobBoss32` |
| `TableName` | Table to open | `Job_Operations` |
| `FieldName` | Field to write | `Note_Text` |
| `table_name` | Table for UPDATE | `Jobs` |
| `Field_name` | Field to update | `Status` |

### 6.7.3 Reading Job Data into Excel (Practical Example)

```vba
Sub GetActiveJobs()
    Dim cn As New ADODB.Connection
    Dim rs As New ADODB.Recordset
    Dim sql As String
    Dim row As Integer

    cn.Provider = "sqloledb"
    cn.ConnectionString = "Data Source=SERVERNAME;Initial Catalog=JobBoss32;Integrated Security=SSPI"
    cn.Open

    sql = "SELECT j.Job, j.Customer, j.Order_Quantity, j.Due_Date " & _
          "FROM Jobs j " & _
          "WHERE j.Status = 'Active' " & _
          "ORDER BY j.Due_Date ASC"

    rs.Open sql, cn, adOpenStatic, adLockReadOnly

    ' Write headers
    Sheets("Jobs").Range("A1").Value = "Job"
    Sheets("Jobs").Range("B1").Value = "Customer"
    Sheets("Jobs").Range("C1").Value = "Qty"
    Sheets("Jobs").Range("D1").Value = "Due Date"

    ' Write data
    row = 2
    Do While Not rs.EOF
        Sheets("Jobs").Cells(row, 1).Value = rs.Fields("Job").Value
        Sheets("Jobs").Cells(row, 2).Value = rs.Fields("Customer").Value
        Sheets("Jobs").Cells(row, 3).Value = rs.Fields("Order_Quantity").Value
        Sheets("Jobs").Cells(row, 4).Value = rs.Fields("Due_Date").Value
        rs.MoveNext
        row = row + 1
    Loop

    rs.Close
    cn.Close
End Sub
```

### 6.7.4 Important Considerations

- **Read-Only vs. Read-Write:** Use `adLockReadOnly` for reporting queries. Only use `adLockOptimistic` when intentionally writing data back to JobBOSS.
- **Data Integrity:** Direct SQL writes bypass JobBOSS business logic (validations, triggers, cascading updates). Use with caution. Test in a non-production environment first.
- **Concurrent Access:** Multiple Excel users opening the same recordset with write locks can cause blocking on the SQL Server.

---

## 6.8 SQL Server Maintenance

### 6.8.1 Index and Statistics Maintenance

As the JobBOSS database grows over time, index fragmentation and outdated statistics degrade query performance. Establish a regular maintenance schedule:

**Recommended Maintenance Tasks:**

| Task | Frequency | Method |
|---|---|---|
| Update Statistics | Weekly | `UPDATE STATISTICS [TableName]` or maintenance plan |
| Rebuild Fragmented Indexes | Monthly | `ALTER INDEX ALL ON [TableName] REBUILD` |
| Reorganize Slightly Fragmented Indexes | Weekly | `ALTER INDEX ALL ON [TableName] REORGANIZE` |
| Check Database Integrity | Weekly | `DBCC CHECKDB('JobBoss32')` |
| Full Database Backup | Daily | SQL Server Backup job or maintenance plan |
| Transaction Log Backup | Every 1-4 hours | SQL Server Backup job (Full/Bulk-Logged recovery model) |

**Check Index Fragmentation:**

```sql
SELECT
    OBJECT_NAME(ind.OBJECT_ID) AS TableName,
    ind.name AS IndexName,
    indexstats.avg_fragmentation_in_percent
FROM
    sys.dm_db_index_physical_stats(DB_ID(), NULL, NULL, NULL, NULL) indexstats
    INNER JOIN sys.indexes ind
        ON ind.object_id = indexstats.object_id
        AND ind.index_id = indexstats.index_id
WHERE
    indexstats.avg_fragmentation_in_percent > 10
ORDER BY
    indexstats.avg_fragmentation_in_percent DESC
```

### 6.8.2 Backup Strategy

> **Critical for Die-Mension:** The JobBOSS database contains all job history, customer data, and financial records. Loss of this data without a current backup is catastrophic.

**Minimum Backup Requirements:**
- Full backup once daily (after business hours)
- Backup stored on a separate physical device or network share from the SQL Server
- At least one offsite or cloud copy (e.g., Backblaze B2, Azure Blob Storage)
- Test restore procedure quarterly

### 6.8.3 Version Compatibility

- SQL Server 2017 (Enterprise, Standard, or Express 64-bit) is confirmed compatible with JobBOSS 12.x
- When upgrading SQL Server, always verify compatibility with the installed JobBOSS version on the ECI customer portal before proceeding
- Express edition limitations: 10 GB max database size, no SQL Agent, no Always On Availability Groups

---

## 6.9 SQL Server Network Configuration

For JobBOSS clients to connect to the SQL Server, the following network configuration must be correct.

### 6.9.1 SQL Server Configuration Manager Settings

1. Open **SQL Server Configuration Manager** on the server
2. Navigate to **SQL Server Network Configuration → Protocols for [InstanceName]**
3. Ensure the following are **Enabled:**
   - **Named Pipes** — Must be Enabled
   - **TCP/IP** — Must be Enabled
4. Navigate to **SQL Server Services**
5. Ensure **SQL Server Browser** service is set to **Running** and **Automatic** startup

### 6.9.2 Authentication Methods

| Method | Recommendation | Notes |
|---|---|---|
| Windows Authentication | Recommended | Uses Active Directory / Windows domain credentials; more secure; no password in connection string |
| SQL Server Authentication | Supported | Requires SQL login; password must be managed and rotated; acceptable for reporting-only accounts |

### 6.9.3 Manual Server Entry Format

When configuring clients that cannot auto-discover the SQL Server instance, use the following manual entry format:

```
192.168.1.100\SQL Server
```

Where:
- `192.168.1.100` = IP address of the SQL Server machine
- `SQL Server` = the name of the SQL Server instance

For the default instance, the instance name portion may be omitted:

```
192.168.1.100
```

### 6.9.4 Firewall Configuration

Ensure the following ports are open on any firewall between clients and the SQL Server:

| Port | Protocol | Purpose |
|---|---|---|
| 1433 | TCP | Default SQL Server port (default instance) |
| 1434 | UDP | SQL Server Browser service (for named instances) |
| Dynamic port | TCP | Named instance port (check in Config Manager) |

---

## 6.10 Database Design Notes

### 6.10.1 The Address Table Double-Key Issue

The `Address` table in JobBOSS has a known design flaw that affects foreign key relationships and query reliability.

**The Problem (verbatim from StevenMcNutt.com):**

> "JobBOSS has a variety of faults, but none more perplexing than their 'double keying' of tables. Foreign key constraints reference `[Address].[Address]` rather than `[Address].[AddressKey]`. This second key is allowed to be null within the database."

**Implications for Die-Mension:**

1. **JOIN Reliability:** When joining to the `Address` table from other tables, be aware that the referenced key (`Address.Address`) is a string field, not the true primary key (`Address.AddressKey`).
2. **Null Values:** The `AddressKey` field (which would normally be the primary key) is allowed to be null, meaning some address records may have no reliable unique identifier.
3. **Duplicate Risk:** Because the join key is a string rather than a GUID or integer identity, duplicate address entries may exist that share the same `Address` value.
4. **Workaround:** Always test address-related queries carefully. Validate that your JOIN to `Address` returns the expected number of rows — check for both missing records (nulls) and duplicate records (multiple matches).

### 6.10.2 GUID Patterns

JobBOSS uses GUIDs (Globally Unique Identifiers, stored as `uniqueidentifier` in SQL Server) as primary keys in many tables. Key patterns to know:

- `RowPointer` — The GUID primary key on most tables; used as the reliable unique identifier for notes linkage and other cross-table references
- `ObjectID` — Used as the primary GUID in the `Bill_Of_Jobs` table
- `Root_Job_OID`, `Parent_Job_OID`, `Component_Job_OID` — GUID references in `Bill_Of_Jobs` linking to job records

**GUID Generation in SQL:**

When inserting new records directly via SQL (e.g., notes), generate new GUIDs using:

```sql
NEWID()
```

Example usage in the notes insertion pattern is shown in Section 6.11.

### 6.10.3 Notes System Architecture Summary

The three-table notes architecture (described in 6.4.3) requires careful handling:
- `NoteHeaders` — Do not modify; it is the system lookup table mapping token types to table names
- `SpecificNotes` — Contains the actual text; safe to insert with proper field values
- `ObjectNotes` — The linking record; must be created after `SpecificNotes` and must reference both the parent record's `RowPointer` and the new `SpecificNoteToken`

---

## 6.11 SQL Query Examples for Common Reports

### 6.11.1 Active Jobs with Priority Filter (LINQ Pattern)

The following LINQ query pattern is documented from JobBOSS developer resources:

```
from op in Job_Operations
join j in Jobs on op.Job equals j.Job
join m in Materials on op.Material1 equals m.Part_Number
where op.Status == "T"
where j.Priority < 9
orderby op.Sequence
select ...
```

**Equivalent SQL:**

```sql
SELECT
    j.Job,
    j.Customer,
    j.Due_Date,
    op.Sequence,
    op.Description,
    op.Work_Center,
    op.Est_Setup_Hrs,
    op.Est_Unit_Cost,
    m.Part_Number,
    m.Description AS Material_Description
FROM
    Job_Operations op
    INNER JOIN Jobs j ON op.Job = j.Job
    INNER JOIN Materials m ON op.Material1 = m.Part_Number
WHERE
    op.Status = 'T'
    AND j.Priority < 9
ORDER BY
    op.Sequence
```

**Filter Reference:**

| Filter | Field | Value | Meaning |
|---|---|---|---|
| Active operations | `op.Status` | `'T'` | Operations with status "T" |
| Priority filter | `j.Priority` | `< 9` | Exclude low-priority or template jobs |
| Sequence order | `op.Sequence` | ASC | Operations in routing order |

### 6.11.2 Inserting Work Instructions (Notes) via SQL

To insert a work instruction note into the `SpecificNotes` table:

```sql
INSERT INTO [SpecificNotes] (
    SpecificNoteToken, NoteContent, NoteDesc, NoteExistsFlag,
    CreatedBy, UpdatedBy, CreateDate, RecordDate, RowPointer, InWorkflow
) VALUES (
    NEWID(), 'note text here', 'WORK INSTRUCTIONS', 0,
    'username', 'username', GETDATE(), GETDATE(), NEWID(), 0
)
```

To link that note to a record via `ObjectNotes`:

```sql
INSERT INTO [ObjectNotes] (
    ObjectNoteToken, NoteHeaderToken, RefRowPointer, NoteType,
    SpecificNoteToken, NoteExistsFlag, CreatedBy, UpdatedBy,
    CreateDate, RecordDate, RowPointer, InWorkflow
)
VALUES (
    NEWID(),                    -- ObjectNoteToken: new GUID
    '[NoteHeaderToken value]',  -- From NoteHeaders table for the target table
    '[Parent Record RowPointer]', -- RowPointer of the Job_Operations or Job record
    '[NoteType]',               -- Note type code
    '[SpecificNoteToken]',      -- Token from the SpecificNotes INSERT above
    0,                          -- NoteExistsFlag
    'username', 'username',     -- CreatedBy, UpdatedBy
    GETDATE(), GETDATE(),       -- CreateDate, RecordDate
    NEWID(), 0                  -- RowPointer, InWorkflow
)
```

> **Note:** Retrieve the correct `NoteHeaderToken` from the `NoteHeaders` table by querying for the row corresponding to the target table name before inserting into `ObjectNotes`.

### 6.11.3 Multi-Level Job Query Using Bill_Of_Jobs

```sql
SELECT
    boj.Top_Lvl_Job,
    boj.Parent_Job,
    boj.Component_Job,
    j.Customer,
    j.Order_Quantity,
    j.Due_Date,
    j.Status
FROM
    Bill_Of_Jobs boj
    INNER JOIN Jobs j ON boj.Component_Job = j.Job
WHERE
    boj.Top_Lvl_Job = 'JOB-NUMBER-HERE'
ORDER BY
    boj.Parent_Job,
    boj.Component_Job
```

> **Reminder:** Use the `Top_Lvl_Job` varchar field (not GUIDs) for consistent results when filtering by top-level job.

### 6.11.4 Job Operations Detail Report

```sql
SELECT
    op.Job,
    op.Sequence,
    op.Description,
    op.Work_Center,
    op.Operation_Service,
    op.Est_Setup_Hrs,
    op.Run,
    op.Run_Method,
    op.Est_Unit_Cost,
    op.Cost_Unit,
    op.Vendor,
    op.WC_Vendor,
    op.Inside_Oper,
    op.Note_Text,
    op.Last_Updated,
    op.Last_Updated_By
FROM
    Job_Operations op
WHERE
    op.Job = 'JOB-NUMBER-HERE'
ORDER BY
    op.Sequence
```

---

## 6.12 JobBOSS SDK

### 6.12.1 SDK Overview

The JobBOSS Software Development Kit (SDK) enables programmatic interaction with JobBOSS data and business logic through a defined interface layer, rather than direct SQL access.

- **SDK URL:** https://resource.ecisolutions.com/jobboss/jobboss-resources/jobboss_softwaredevk
- **Architecture:** XML-based request processing through a COM object (`JBInterface.exe`)
- **Publisher:** ECI Solutions (formerly Exact Software)

### 6.12.2 COM Object Architecture

The SDK operates through `JBInterface.exe`, which:
1. Accepts XML-formatted requests
2. Processes them through JobBOSS business logic
3. Returns XML-formatted responses

Each request requires a valid **SessionID**, which is obtained by creating a session with the `JBRequestProcessor` COM object.

### 6.12.3 .NET Integration

For .NET applications (C#, VB.NET), the COM object must be converted to a .NET-compatible library:

```
tlbimp JBInterface.exe /out:JBRequestProcessorNet.dll
```

This produces `JBRequestProcessorNet.dll` which can be referenced directly from .NET projects.

### 6.12.4 C# Pattern

```csharp
// Step 1: Create session
var processor = new JBRequestProcessor();
string sessionID = processor.CreateSession(username, password, companyCode);

// Step 2: Build XML request
string xmlRequest = $@"<Request>
    <SessionID>{sessionID}</SessionID>
    <RequestType>GetJob</RequestType>
    <JobNumber>JOB-12345</JobNumber>
</Request>";

// Step 3: Process request
string xmlResponse = processor.ProcessRequest(xmlRequest);

// Step 4: Parse XML response
// ... parse xmlResponse for returned data
```

### 6.12.5 Licensing Considerations

> **Important:** Each user (connection) to the JobBOSS SDK requires a **separate seat license**. When building integrations that use the SDK, each concurrent connection counts as one JobBOSS user seat. Plan the integration architecture accordingly to minimize concurrent connections.

**Implications for Die-Mension:**
- A background integration service that maintains a persistent connection uses one seat continuously
- Design SDK integrations to open a session, perform operations, and close the session promptly
- Batch operations during off-hours to minimize seat usage during business hours

---
---

# SECTION 7: API AND INTEGRATION GUIDE

---

## 7.1 Overview of Integration Options

Die-Mension has access to multiple integration pathways for connecting JobBOSS/JobBOSS² to external systems. Each method has different technical requirements, capabilities, maintenance burden, and licensing implications.

**Integration Method Summary:**

| Method | Best For | Technical Level | Requires License |
|---|---|---|---|
| Direct SQL / ODBC | Custom reporting, data extraction, Excel dashboards | SQL knowledge required | No additional license |
| VBA / Excel ADO | Internal reports, one-off data pulls | VBA/SQL knowledge | No additional license |
| Crystal Reports | Formatted printable reports via JobBOSS interface | Crystal Reports skill | Crystal Reports license |
| JobBOSS SDK | Programmatic read/write with business logic enforcement | Developer | Per-seat SDK license |
| JobBOSS² Public REST API | Cloud integrations, modern web apps | REST/OAuth2 developer | Included with JobBOSS² |
| Makini Unified API | Multi-system manufacturing data integration | REST developer | Makini subscription |
| Paperless Parts | Quoting-to-ERP bidirectional integration | Configuration-based | Paperless Parts subscription |
| CAD BOM Import | SolidWorks/Inventor/Excel BOM entry automation | Minimal — file-based | Included (ECI download) |
| Commercient SYNC | CRM and eCommerce sync | Configuration-based | Commercient subscription |
| KnowledgeSync | KPI monitoring and automated alerts | Configuration-based | Included (ECI acquisition) |
| QuickBooks Integration | Accounting/GL sync | Configuration-based | QuickBooks subscription |
| Alora | Machine monitoring / OEE | Hardware + software | Alora subscription |
| Costimator | Cost estimating with JobBOSS data | Configuration-based | Costimator license |
| NET1 | Card and ACH payment processing | Configuration-based | NET1 subscription |
| CenterPoint Payroll | Payroll data sync | Configuration-based | CenterPoint license |

---

## 7.2 JobBOSS² Public REST API

### 7.2.1 Overview

JobBOSS² includes a **public REST API** that enables custom integrations to read and write data using standard HTTP methods. This is the preferred modern integration method for web applications, cloud services, and automated data pipelines.

### 7.2.2 Authentication

The JobBOSS² API uses **OAuth2 Bearer Token** authentication:

| Credential | Description |
|---|---|
| `client_id` | Unique identifier for your integration application |
| `client_secret` | Secret key for your integration application |

**Authentication Flow:**
1. Request a bearer token from the JobBOSS² OAuth2 token endpoint using `client_id` and `client_secret`
2. Include the bearer token in all subsequent API requests as an HTTP Authorization header:
   ```
   Authorization: Bearer {your_token_here}
   ```
3. Refresh the token before it expires (token expiration varies; check the API documentation)

**Obtaining API Credentials:**
- Log in to the JobBOSS² customer portal at https://customers.jobboss.com
- Navigate to API/Integration settings to generate `client_id` and `client_secret`
- Store these credentials securely (do not commit to source code repositories)

### 7.2.3 Power BI Integration via REST API

Power BI Desktop can connect directly to the JobBOSS² web API using the `client_id` and `client_secret` credentials:

1. Open Power BI Desktop
2. Click **Get Data → Web**
3. Enter the JobBOSS² API endpoint URL
4. When prompted for authentication, provide `client_id` and `client_secret`
5. Power BI will fetch the data and allow table/report creation

This enables live or scheduled-refresh dashboards in Power BI that pull directly from JobBOSS² without requiring direct database access.

### 7.2.4 Capabilities

The JobBOSS² REST API supports creating custom integrations that can:
- Read job data, customer records, material records, and routing
- Create and update records with business logic enforcement (unlike direct SQL writes)
- Trigger actions within JobBOSS²
- Support bidirectional data flow with external systems

> **Advantage over Direct SQL:** Using the REST API enforces JobBOSS² business logic and validation rules. Direct SQL writes bypass these rules and can result in data integrity issues. Use the API for any integration that writes data back to JobBOSS².

---

## 7.3 Makini Unified Manufacturing API

### 7.3.1 Overview

Makini provides a **unified manufacturing API layer** that standardizes data from multiple ERP and MES systems — including JobBOSS — into a consistent set of entities. This is useful when Die-Mension needs to integrate JobBOSS with systems that support the Makini standard.

### 7.3.2 Access Details

| Item | Value |
|---|---|
| Documentation URL | https://makini.stoplight.io/docs/api-docs/docs/getting-started.md |
| Base API URL | https://api.makini.io |

### 7.3.3 Supported Manufacturing Data Entities

Makini supports 30+ standardized manufacturing data entities, including:

| Entity | Description |
|---|---|
| Organization | Company and facility records |
| Work Order | Manufacturing work order data |
| Purchase Order | Procurement order data |
| Receipt | Receiving records |
| Resource | Machine, work center, and labor resource data |
| Site | Plant/facility location data |
| (+ 25 more) | Additional entities per Makini documentation |

### 7.3.4 Use Cases for Die-Mension

- Connecting JobBOSS data to a third-party scheduling or analytics platform that supports the Makini API standard
- Building integrations that remain ERP-agnostic (easier to switch ERP systems in the future without rebuilding integrations)
- Aggregating data from multiple manufacturing systems into a single unified data layer

---

## 7.4 QuickBooks Integration

### 7.4.1 Overview

JobBOSS² provides a direct integration with QuickBooks Online for accounting data synchronization. This eliminates manual re-entry of invoice and financial data between the two systems.

**Documented Versions:** v5.3 and v5.5

### 7.4.2 Data Flow

The QuickBooks integration is primarily **one-directional** from JobBOSS² into QuickBooks:

```
JobBOSS² → QuickBooks Online
```

**Sync can be configured as:**
- **Real-time sync** — Data flows to QuickBooks immediately upon entry in JobBOSS²
- **Batch process** — Data is accumulated and synced on a schedule

### 7.4.3 Data Objects Synced

| Data Object | Synced to QuickBooks |
|---|---|
| Invoices | Yes |
| Customers | Yes |
| Vendors | Yes |
| Payments | Yes |
| (Others per configuration) | Review with ECI support |

### 7.4.4 Known Limitations and Unsupported Features

The following items are **NOT supported** or have known limitations in the QuickBooks Online integration:

| Item | Status | Notes |
|---|---|---|
| Sales Tax | NOT synced | Must be managed separately in QuickBooks Online |
| Payment Terms | NOT synced | Must be configured separately in QuickBooks Online |
| Vendor Cash Discounts | NOT supported | Not available in the QuickBooks Online version of the integration |
| Direct AR Journal Entries | NOT supported | Cannot do a journal entry directly to the AR account in QuickBooks Online; must import as a detailed transaction |
| G/L Account Name Uniqueness | Required | G/L account names must be unique in QuickBooks Online; duplicate names will cause sync errors |

> **Action Item for Die-Mension:** Before activating the QuickBooks integration, audit QuickBooks Online G/L account names to ensure all are unique. Duplicate account names are a common cause of integration failures.

### 7.4.5 QuickBooks Online AR Entry Rule

A specific architectural constraint applies to Accounts Receivable:

**Rule:** You cannot post a journal entry directly to the AR account in QuickBooks Online.

**Workaround:** All AR transactions from JobBOSS² must be imported as **detailed transactions** (individual invoices or payments), not as summarized journal entries.

### 7.4.6 Setup and Configuration

Configuration of the QuickBooks integration is performed within JobBOSS² under the accounting integration settings. Contact ECI Solutions support for version-specific setup documentation for v5.3 or v5.5.

---

## 7.5 Paperless Parts Integration

### 7.5.1 Overview

Paperless Parts is a **cloud-based quoting platform** for precision job shops and contract manufacturers. The bidirectional integration between Paperless Parts and JobBOSS ERP eliminates double data entry when quotes convert to orders.

- **Partnership established:** October 2022
- **Available for:** All JobBOSS and JobBOSS² customers

### 7.5.2 Bidirectional Data Flow

**FROM JobBOSS INTO Paperless Parts:**

| Data | Direction | Purpose |
|---|---|---|
| Customer list | JobBOSS → Paperless Parts | Auto-populate customer on quotes |
| Raw material pricing | JobBOSS → Paperless Parts | Use actual material costs in quote pricing |
| Work center rates | JobBOSS → Paperless Parts | Use actual labor/machine rates in quote pricing |

**FROM Paperless Parts INTO JobBOSS²:**

| Data | Direction | Purpose |
|---|---|---|
| BOM (Bill of Materials) | Paperless Parts → JobBOSS² | Auto-create material requirements on job |
| Routing information | Paperless Parts → JobBOSS² | Auto-create operation routing on job |
| All order information | Paperless Parts → JobBOSS² | Complete job creation upon order placement |

### 7.5.3 Business Impact

- **Time savings:** Eliminates approximately **8 hours per week** in double data entry
- When a quote converts to an order in Paperless Parts, the complete job is automatically created in JobBOSS² including BOM and routing
- Customer data stays synchronized between both systems

### 7.5.4 Relevance for Die-Mension

As a precision tool and die shop, Die-Mension regularly quotes custom work requiring detailed routing and material specifications. The Paperless Parts integration is particularly valuable because:
- CAD-driven quoting in Paperless Parts feeds directly into JobBOSS² job creation
- Work center rates from JobBOSS² ensure quote accuracy reflects actual shop rates
- The 8 hours/week savings applies to shops of Die-Mension's size

---

## 7.6 CAD BOM Import

### 7.6.1 Overview

The **CAD Bill of Material Import** utility automates the entry of BOM data from CAD systems and Excel into the material and component tabs of JobBOSS² jobs or job templates.

- **Download URL:** https://customers.jobboss.com/software-downloads-2/other-software-downloads/cad-bill-of-material-import

### 7.6.2 Supported BOM Structures

| Structure Type | Supported |
|---|---|
| Single-level BOM | Yes |
| Multi-level BOM | Yes |

### 7.6.3 Supported Input Formats

| Format | Notes |
|---|---|
| SolidWorks BOM export | Direct import supported |
| Inventor BOM export | Direct import supported |
| Excel spreadsheet | Minimum 3 fields required (see below) |

### 7.6.4 Excel Import Requirements

When importing from Excel, only **3 fields are required:**

| Required Field | Description |
|---|---|
| Part # | Part number identifier |
| Description | Part description |
| Qty | Quantity required |

Additional fields from the Excel file will be mapped if present, but these three are the minimum required for a successful import.

### 7.6.5 Material Pre-Existence

> **Important:** Materials do **NOT** have to pre-exist in the JobBOSS² material library. The CAD BOM Import tool can **automatically create new part numbers** in the material library during the import process.

This is a significant advantage over manual entry or SQL imports that require pre-existing material records.

### 7.6.6 Use Cases for Die-Mension

- Importing a SolidWorks assembly BOM directly into a new JobBOSS² job, creating all material line items automatically
- Importing an Excel BOM from a customer-supplied drawing package
- Rapidly setting up multi-level jobs for complex die assemblies where the component list is defined in CAD

---

## 7.7 Commercient SYNC (CRM and eCommerce Integrations)

### 7.7.1 Overview

**Commercient SYNC** is a cloud-based integration platform that connects JobBOSS to CRM, eCommerce, and analytics systems. It handles ongoing data synchronization without requiring custom development.

### 7.7.2 Supported CRM Platforms

| CRM Platform | Integration Available |
|---|---|
| Salesforce | Yes |
| SAP CRM | Yes |
| Microsoft Dynamics 365 | Yes |
| Zoho CRM | Yes |
| HubSpot | Yes |

### 7.7.3 Supported eCommerce and Other Platforms

| Platform | Category |
|---|---|
| WooCommerce | eCommerce |
| SPS Commerce | eCommerce / EDI |
| Tableau | Analytics |
| ZenDesk | Customer Support |

### 7.7.4 Data Objects Synced

The following data objects are synchronized between JobBOSS and the connected platform:

| Data Object | Description |
|---|---|
| Accounts | Customer and company records |
| Orders | Sales orders and job orders |
| Contacts | Individual contact records |
| Products | Product and part catalog data |
| Inventory levels | Current stock quantities |
| Invoices | Invoice records |
| Payment statuses | Current payment state on invoices |
| Past order history | Historical order data |
| Outstanding invoices | Open/unpaid invoice data |

### 7.7.5 Sync Frequency

| Sync Speed | Range |
|---|---|
| Minimum frequency | Once per day |
| Maximum frequency | Near real-time |

Sync frequency is configurable based on the Commercient subscription tier and business requirements.

### 7.7.6 Use Cases for Die-Mension

- Syncing JobBOSS customer accounts and order history into HubSpot or Salesforce CRM for sales team visibility
- Pushing invoice and payment status data to a CRM to enable customer-facing account visibility
- Syncing product/part catalog from JobBOSS to a WooCommerce store (if Die-Mension sells standard tooling items online)

---

## 7.8 KnowledgeSync Alerting

### 7.8.1 Overview

**KnowledgeSync** (by Vineyardsoft) is an automated monitoring and alerting platform that integrates with JobBOSS to watch key performance indicators (KPIs) and send notifications when thresholds are met or conditions change.

- **ECI acquired Vineyardsoft in September 2018**, making KnowledgeSync part of the ECI Solutions product family
- KnowledgeSync is available to JobBOSS customers through ECI

### 7.8.2 How It Works

KnowledgeSync continuously monitors the JobBOSS database (and other connected data sources) against user-defined rules. When a monitored condition is triggered, it automatically sends alerts via email, SMS, or other configured channels.

**Monitoring Cycle:**
```
[KnowledgeSync monitors JobBOSS data]
        |
        v
[Threshold or condition met]
        |
        v
[Automated alert sent to defined recipients]
```

### 7.8.3 Confirmed Use Case Example

> **Inventory Alert Example:** If inventory for a tracked material drops below a defined minimum level, KnowledgeSync immediately notifies the purchasing team — without requiring anyone to manually check inventory levels.

### 7.8.4 Additional Use Cases for Die-Mension

| KPI to Monitor | Trigger Condition | Alert Recipient |
|---|---|---|
| Past-due jobs | Due date passed, status not Complete | Shop manager, scheduler |
| Low raw material inventory | Stock quantity below reorder point | Purchasing |
| Job cost overrun | Actual cost exceeds estimated cost by X% | Operations manager |
| Customer order aging | Invoice unpaid beyond terms + N days | Accounting |
| Machine downtime (with Alora) | Machine offline beyond threshold | Production manager |
| Quote conversion | Quote status changes to Order | Sales, scheduling |

### 7.8.5 Support and Documentation

- **Support URL:** https://www.vineyardsoft.com/support_documentation.php

---

## 7.9 Alora Machine Intelligence

### 7.9.1 Overview

**Alora** is a machine monitoring add-on integration for JobBOSS that provides real-time visibility into machine performance on the shop floor by collecting actual machine runtime data and comparing it against JobBOSS estimates.

### 7.9.2 Core Functionality

| Feature | Description |
|---|---|
| Estimated vs. Actual Comparison | Compares Estimated Run Time from JobBOSS against actual Machine Hours recorded by Alora sensors |
| Machine Performance Visibility | Dashboards showing real-time and historical machine performance |
| Uptime Tracking | Measures productive machine time vs. scheduled time |
| Downtime Tracking | Identifies and categorizes machine downtime events |
| Scrap Rate Tracking | Tracks defective parts/scrap events per machine |

### 7.9.3 Integration Architecture

```
[Shop Floor Machines]
        |
        | (Alora sensors / data collectors)
        v
[Alora Platform]
        |
        | (Integration layer)
        v
[JobBOSS Job_Operations: Est_Setup_Hrs, Run, Est_Unit_Cost]
        |
        v
[Comparison Reports: Estimated vs. Actual]
```

### 7.9.4 Value for Die-Mension

As a precision tool and die shop, accurate machine time estimation is critical for both quoting and scheduling. Alora provides:
- Data to improve future job estimates based on actual performance
- Identification of machines that consistently run over or under estimate
- Objective downtime data for maintenance planning
- OEE (Overall Equipment Effectiveness) metrics per machine

---

## 7.10 Costimator Integration

### 7.10.1 Overview

**MTI Systems Costimator JS** integrates directly with JobBOSS to provide high-accuracy cost estimating that feeds directly into job data.

### 7.10.2 Data Exported from Costimator into JobBOSS

When a Costimator estimate is completed and approved, the following data is exported directly into JobBOSS:

| Data Exported | Destination in JobBOSS |
|---|---|
| Machine cycle times | Job_Operations routing |
| Labor costs | Job_Operations cost fields |
| Material usage | Material tab on the job |

### 7.10.3 Capabilities Enabled

| Capability | Description |
|---|---|
| High-accuracy quoting | Cycle times and costs computed by Costimator rather than manual estimation |
| Material inventory management | Material usage data from Costimator keeps JobBOSS material needs current |
| Cost-to-quote traceability | Direct link between the estimate that generated the price and the resulting job data |

### 7.10.4 Use Cases for Die-Mension

- Estimating machining time for progressive die components using Costimator's feature-based costing engine, then pushing those times directly into the JobBOSS job routing
- Ensuring quoted cycle times and the actual routing used for scheduling are identical (eliminating the disconnect between estimating and production)

---

## 7.11 NET1 Payment Processing

### 7.11.1 Overview

**NET1** is a fully integrated payment processing solution for JobBOSS² that allows Die-Mension to accept customer payments directly within the JobBOSS interface.

### 7.11.2 Payment Methods Supported

| Payment Method | Supported |
|---|---|
| Credit card | Yes |
| Debit card | Yes |
| ACH (bank transfer) | Yes |

### 7.11.3 Integration Benefits

| Benefit | Description |
|---|---|
| Faster cash application | Payments post directly to the JobBOSS² invoice without manual re-entry |
| Better financial visibility | Payment status is immediately visible in JobBOSS² without waiting for accounting batch processing |
| Less manual work | Eliminates the manual step of recording payments received outside the system |

### 7.11.4 Architecture

NET1 is **fully integrated** with JobBOSS², meaning payment transactions originate within and post back to JobBOSS² automatically. This is not a separate system that requires manual reconciliation.

---

## 7.12 CenterPoint Payroll Integration

### 7.12.1 Overview

An integration is available between **CenterPoint Payroll** (by Red Wing Software) and JobBOSS². This allows payroll data to synchronize between the two systems, reducing duplicate entry of employee time and payroll information.

### 7.12.2 Reference Documentation

| Resource | Details |
|---|---|
| Integration Document | https://resources.redwingsoftware.com/rwssnextfiles/kbdocs/general/3460%20pr-jobboss2-cp-interface.pdf |
| Publisher | Red Wing Software |
| Integration Version | JobBOSS² compatible |

### 7.12.3 Setup

Download and follow the integration setup document from the Red Wing Software resources URL above. The document covers:
- Configuration steps in both CenterPoint Payroll and JobBOSS²
- Field mapping between the two systems
- Sync process and frequency

---

## 7.13 Integration Selection Guide

Use this guide to select the correct integration method for a given Die-Mension requirement.

### 7.13.1 Decision Matrix

| Requirement | Recommended Method | Reason |
|---|---|---|
| Build a custom printed report for shop floor use | Crystal Reports (Section 6.5) | Native to JobBOSS; professional layout; accessible from JobBOSS menu |
| Pull job data into an Excel workbook for one-time analysis | VBA/Excel ADO (Section 6.7) | Fast, no licensing, direct SQL access |
| Build a live Power BI dashboard for management | JobBOSS² REST API or Direct ODBC | REST API preferred for cloud; ODBC for on-premise Power BI Desktop |
| Sync customers and invoices to Salesforce CRM | Commercient SYNC (Section 7.7) | Pre-built connector; no custom development |
| Automate quoting-to-job creation | Paperless Parts (Section 7.5) | Purpose-built for this workflow; 8 hrs/week savings |
| Import a SolidWorks BOM into a new job | CAD BOM Import (Section 7.6) | Native ECI tool; handles multi-level BOMs; auto-creates materials |
| Get email alerts when jobs go past due | KnowledgeSync (Section 7.8) | Purpose-built monitoring; no coding required |
| Monitor actual vs. estimated machine time | Alora (Section 7.9) | Shop floor machine monitoring purpose-built for this |
| Send invoices to QuickBooks | QuickBooks Integration (Section 7.4) | Native integration; real-time or batch |
| Accept customer credit card payments | NET1 (Section 7.11) | Fully integrated; posts directly to JobBOSS² |
| Sync payroll data from CenterPoint | CenterPoint Integration (Section 7.12) | Pre-built connector per Red Wing documentation |
| Feed cost estimates into job routing | Costimator (Section 7.10) | Purpose-built; exports cycle times and costs directly |
| Write custom application with full business logic | JobBOSS SDK (Section 6.12) | Enforces business rules; XML-based; seat license required |
| Build modern web integration with JobBOSS² | REST API (Section 7.2) | OAuth2; standard REST; Power BI compatible |

### 7.13.2 Direct SQL Access vs. API/SDK: When to Use Each

**Use Direct SQL (ODBC / ADO) When:**
- The requirement is read-only reporting
- Speed of development is more important than strict business logic enforcement
- The user has SQL skills and needs a quick answer
- Building an Excel-based internal report or dashboard

**Use the JobBOSS² REST API When:**
- Writing data back to JobBOSS² (jobs, orders, customers)
- Building a cloud-hosted integration
- The target system supports REST/OAuth2 natively
- Business logic enforcement is required on writes

**Use the SDK When:**
- A .NET or COM-based application needs programmatic read/write access
- Business logic enforcement is required
- The integration needs to trigger JobBOSS workflows, not just write raw data

**Use a Pre-Built Integration (Paperless Parts, Commercient, etc.) When:**
- The use case matches the integration's stated purpose exactly
- Custom development resources are not available
- Time-to-deployment is the priority

### 7.13.3 Data Write Safety Rules

| Write Method | Enforces Business Logic | Risk Level | Recommended For |
|---|---|---|---|
| Direct SQL INSERT/UPDATE | No | High | Read-only reporting only; avoid writes |
| JobBOSS SDK | Yes | Low | Programmatic writes |
| REST API | Yes | Low | Modern integrations |
| Pre-built integrations | Typically yes | Low | Purpose-specific use cases |

> **Rule for Die-Mension:** Never write to the JobBOSS database directly via SQL unless the operation has been thoroughly tested in a non-production copy of the database. Always prefer the SDK or REST API for any operation that modifies data.

---

## 7.14 Key Resource URLs

The following URLs are reference points for Die-Mension staff and AI assistants working with JobBOSS integrations and technical resources.

| Resource | URL |
|---|---|
| ECI/JobBOSS Customer Portal | https://customers.jobboss.com |
| Data Dictionary (doc 27.240.736) | https://customers.jobboss.com → Release and Download Info → Knowledge Base → Utility and Other → Utilities |
| JobBOSS SDK | https://resource.ecisolutions.com/jobboss/jobboss-resources/jobboss_softwaredevk |
| CAD BOM Import Utility | https://customers.jobboss.com/software-downloads-2/other-software-downloads/cad-bill-of-material-import |
| Makini API Documentation | https://makini.stoplight.io/docs/api-docs/docs/getting-started.md |
| Makini Base API | https://api.makini.io |
| KnowledgeSync / Vineyardsoft Support | https://www.vineyardsoft.com/support_documentation.php |
| CenterPoint Payroll / JobBOSS² Integration Doc | https://resources.redwingsoftware.com/rwssnextfiles/kbdocs/general/3460%20pr-jobboss2-cp-interface.pdf |

### 7.14.1 Additional Reference Resources (Public Technical Sources)

| Resource | Notes |
|---|---|
| jobbossuser.com | Community user forum; Bill_Of_Jobs field documentation; SDK patterns |
| StevenMcNutt.com | Documented Address table double-key design issue |
| SAP Community | Crystal Reports performance optimization documentation |
| MrExcel forum | VBA/ADO connection patterns for SQL Server |
| makini.io | Unified manufacturing API documentation |

---

*End of Section 6 and Section 7*

*Document: JobBOSS / JobBOSS² Master Reference Manual — Die-Mension Tool & Die, Brunswick, OH*
*Sections: 6 (Database and Technical Architecture) and 7 (API and Integration Guide)*
*Prepared for use by internal staff and AI assistants supporting technical configuration, custom reporting, and system integrations.*
```

---

Here is a summary of what was written and how the source data was used:

**Section 6: Database and Technical Architecture** covers 12 subsections across approximately 900 lines. All verbatim research data was incorporated:

- SQL Server 2017 compatibility table, database names (`JobBoss32` / `production`), and architecture diagram in 6.1
- Exact customer portal navigation path and document number 27.240.736 in 6.2
- All 11 confirmed table names with descriptions in 6.3
- `Bill_Of_Jobs` field table verbatim, notes three-table architecture, and `Job_Operations` join keys in 6.4
- The 7-step Crystal Reports workflow with the verbatim best practice quotes, the 4-minute performance problem fix, and the `UD_Report.htm` dual-directory requirement in 6.5
- All four ODBC connection string variants verbatim in 6.6
- The complete VBA/ADO code block verbatim in 6.7
- Maintenance tasks, backup strategy, and version compatibility in 6.8
- Named Pipes, TCP/IP, SQL Server Browser, and IP entry format verbatim in 6.9
- The verbatim StevenMcNutt.com double-key quote and GUID pattern documentation in 6.10
- The LINQ query verbatim converted to equivalent SQL, the `SpecificNotes` and `ObjectNotes` INSERT statements verbatim, and a `Bill_Of_Jobs` query in 6.11
- SDK URL, `JBInterface.exe`/`tlbimp` pattern, C# session/ProcessRequest pattern, and per-seat licensing note in 6.12

**Section 7: API and Integration Guide** covers 14 subsections across approximately 800 lines. All verbatim integration data was incorporated:

- Complete integration method decision table in 7.1
- OAuth2 `client_id`/`client_secret` authentication and Power BI connection in 7.2
- Makini URL, base API, and all 6+ listed entity types in 7.3
- QuickBooks limitations table verbatim (Sales Tax, Payment Terms, Vendor Cash Discounts, AR journal entry rule, G/L uniqueness requirement), versions v5.3 and v5.5 in 7.4
- Bidirectional Paperless Parts data flow table, 8 hours/week savings, October 2022 partnership in 7.5
- CAD BOM Import URL, supported formats, 3 required Excel fields, auto-create materials capability in 7.6
- All 5 CRMs, all 4 other platforms, all 9 data objects, sync frequency range verbatim in 7.7
- ECI/Vineyardsoft acquisition date (September 2018), inventory alert example verbatim, support URL in 7.8
- Estimated vs. actual comparison, uptime/downtime/scrap tracking verbatim in 7.9
- Machine cycle times, labor costs, material usage export verbatim in 7.10
- Card, ACH, faster cash application, financial visibility verbatim in 7.11
- Red Wing Software URL verbatim in 7.12
- Decision matrix covering all integration methods in 7.13
- All URLs from the research data collected in 7.14


---

# SECTION 8: REPORTING GUIDE

## 8.1 Overview: Reporting Architecture in JobBOSS

JobBOSS and JobBOSS² support two distinct reporting engines depending on deployment type. Understanding which engine is in use and what each can and cannot do is essential before attempting any reporting customization.

| Attribute | Legacy JobBOSS (On-Premise) | JobBOSS² (Cloud) |
|---|---|---|
| Primary Report Engine | SAP Crystal Reports (.rpt files) | Stimulsoft (embedded) |
| Secondary Engine | N/A | Crystal Reports still supported |
| Database Access | Direct SQL Server via ODBC | SQL Server via ODBC or API |
| Customization Difficulty | Moderate (requires Crystal license) | High (Stimulsoft embedded, limited access) |
| Support Path for Custom Reports | SAP Crystal Reports support + ECI | ECI only (cannot contact Stimulsoft directly) |
| Third-Party Resources | ReportingGuru.com | ReportingGuru.com (Crystal/API) |

### Core Architecture Principles

Both legacy JobBOSS and JobBOSS² reports ultimately connect to the same underlying SQL Server database. All reporting — whether Crystal Reports, Stimulsoft, Excel ODBC, or Power BI — reads from this database. This means:

- SQL query knowledge is universally valuable across all reporting methods.
- ODBC configuration (Section 6.6) is the foundation for all custom reporting.
- Data integrity in the source tables determines the accuracy of every report. Garbage in, garbage out.

### The Support Gap Problem

> **CRITICAL SUPPORT NOTE:** Stimulsoft is an embedded OEM component within JobBOSS². Stimulsoft's own support team cannot assist JobBOSS² customers because those customers are technically customers of ECI, not Stimulsoft. If a Stimulsoft report in JobBOSS² behaves incorrectly, crashes, or produces wrong output, the only valid support path is ECI support. ECI developers have access to the embedded Stimulsoft configuration and are the only ones who can modify it at the platform level.

### Third-Party Reporting Resource

**ReportingGuru.com** is an independent vendor specializing in custom JobBOSS reporting. They have over 15 years of experience building Crystal Reports for JobBOSS customers. Services include:

- Custom Crystal Reports development
- SQL Server Reporting Services (SSRS) reports
- JobBOSS² Cloud API-based reports
- Report debugging and performance optimization

For Die-Mension, ReportingGuru.com is the recommended resource when ECI's built-in reports are insufficient and internal Crystal Reports skills are not available.

---

## 8.2 Standard (Built-In) Reports Reference

The following tables describe every major built-in report available in JobBOSS, organized by module. These reports require no additional software or configuration to run — they are accessible directly from the Reports menu within the application.

### 8.2.1 Job and Production Reports

| Report Name | What It Shows | When to Run | Key Fields |
|---|---|---|---|
| Job Cost Report | Estimated vs. actual costs per job broken down by labor, material, outside processing, and overhead | After job closes or on demand | Job number, customer, part, est. cost, actual cost, variance |
| Job Status Report | Current status of all open jobs (In Process, On Hold, Released, Completed) | Daily | Job number, part, customer, status, due date |
| Job List | All jobs with customer, part number, quantity, due date, and status | Daily or as needed | Job number, customer, part, qty, due date, status |
| Job Traveler | Shop packet printed per individual job; includes routing steps, materials list, special instructions, quantity, and job number | At job release | All routing, materials, notes |
| Job Profitability Report | Revenue vs. total cost per job, showing gross profit and margin percentage | Weekly | Job number, revenue, total cost, gross profit, margin % |
| Estimated vs. Actual Report | Side-by-side comparison of estimated and actual labor hours, material cost, and outside processing across multiple jobs | Monthly | Job number, operation, est. hrs, actual hrs, est. cost, actual cost |
| Open Jobs Report | All jobs that have not yet been shipped or closed | Daily | Job number, part, customer, qty, due date, status |
| Jobs Behind Schedule Report | Jobs whose projected completion date exceeds the due date | Daily | Job number, customer, due date, projected date, days late |
| Work Center Load Report | Capacity loading by work center, showing scheduled hours vs. available hours by day or week | Weekly | Work center, date, scheduled hours, available hours, utilization % |
| On-Time Delivery Report | Percentage of jobs shipped on or before the due date, by customer or date range | Monthly | Customer, job number, due date, ship date, on time Y/N |

> **DIE-MENSION NOTE:** The **Estimated vs. Actual Report** is the single most important report for continuous improvement at a job shop. Run it monthly. Use it to find operations where actual hours consistently exceed estimates — those operations need re-pricing or process improvement.

### 8.2.2 Quoting and Estimating Reports

| Report Name | What It Shows | When to Run | Key Fields |
|---|---|---|---|
| Quote List | All open quotes with customer, part, status, quoted price, and expiration date | Weekly | Quote number, customer, part, quoted price, status, expiration |
| Expired Quotes Report | Quotes that passed their validity date without being converted to a job or marked lost | Weekly | Quote number, customer, part, expiration date, days expired |
| Quote Win/Loss Report | Converted vs. lost quotes by customer, with reason codes if entered | Monthly | Customer, quote count, won, lost, win rate %, lost reason |

### 8.2.3 Purchasing Reports

| Report Name | What It Shows | When to Run | Key Fields |
|---|---|---|---|
| Open PO Report | All purchase orders that have not been fully received, with vendor, expected delivery, and quantities outstanding | Daily | PO number, vendor, part, qty ordered, qty received, qty outstanding, due date |
| Receiving History Report | All material receipts during a specified date range | On demand | PO number, vendor, part, qty received, date, lot number |
| On-Order Report | All materials currently on open purchase orders, summarized by part number | Weekly | Part number, description, qty on order, vendor, expected date |
| Vendor Performance Report | On-time delivery percentage by vendor for a date range | Monthly | Vendor, PO count, on-time %, average days late |
| Cash Requirements Report | Accounts payable payments due within a user-specified future date range | Weekly | Vendor, PO/invoice number, amount due, due date |

### 8.2.4 Inventory and Material Reports

| Report Name | What It Shows | When to Run | Key Fields |
|---|---|---|---|
| Inventory Valuation Report | Total inventory value by part number using the configured costing method (Standard, Average, FIFO) | Monthly or at period end | Part number, description, qty on hand, unit cost, total value |
| Transaction Register Report | All material transactions (receipts, issues, adjustments) for a specified period | On demand or period end | Date, part number, transaction type, qty, cost, user |
| Stale Inventory Report | Parts with no transactions since a user-specified date; identifies dead stock | Quarterly | Part number, description, last transaction date, qty on hand, value |
| Reorder Report | Parts at or below their configured reorder point | Weekly | Part number, description, qty on hand, reorder point, reorder qty |
| Physical Count Worksheets | Blank forms listing all parts expected to be in inventory; used during physical count | At count time | Part number, description, bin location, blank qty field |
| Lot Tracking Report | Complete transaction history for a specific lot number (heat number) from receipt through all usages | On demand for customer/auditor requests | Lot number, part, vendor, receipt date, jobs consumed on, qty used |
| Material Requirements Report | All materials needed for currently open jobs that have not yet been purchased or allocated | Weekly | Part number, description, qty required, qty on hand, qty short |

> **DIE-MENSION NOTE:** The **Lot Tracking Report** is the Die-Mension answer to customer heat number audit trail requests. When a customer calls asking for material certification traceability on a specific part, run this report by lot/heat number to see every job that material was consumed on. This is a regulatory and contractual requirement for aerospace and automotive customers.

### 8.2.5 Financial Reports

| Report Name | What It Shows | When to Run | Key Fields |
|---|---|---|---|
| Accounts Receivable Aging Report | Customer outstanding balances grouped by aging bucket (Current, 30, 60, 90+ days) | Weekly | Customer, invoice number, invoice date, balance, aging bucket |
| Accounts Payable Aging Report | Vendor outstanding balances grouped by aging bucket | Weekly | Vendor, PO/invoice number, amount, aging bucket |
| Invoice Register | All customer invoices generated within a specified period | Monthly | Invoice number, customer, date, amount, status |
| Cash Flow Analysis | Projected cash receipts and disbursements by period | Monthly | Period, projected receipts, projected disbursements, net cash |
| Income Statement / P&L | Revenue, cost of goods sold, gross profit, operating expenses, and net income for a period | Monthly | Revenue, COGS, gross profit, expenses, net income |
| General Ledger Summary | All GL account activity for a period, with beginning balance, debits, credits, and ending balance | Monthly or period end | Account number, account name, beginning balance, debits, credits, ending balance |

### 8.2.6 Scheduling Reports

| Report Name | What It Shows | When to Run | Key Fields |
|---|---|---|---|
| Production Schedule | All open jobs ordered by schedule date, showing work center assignments | Daily | Job number, part, work center, scheduled start, scheduled end, due date |
| Capacity Report | Work center utilization as a percentage of available hours for a date range | Weekly | Work center, period, available hours, scheduled hours, utilization % |
| Late Jobs Report | All jobs whose scheduled completion is past the due date | Daily | Job number, customer, due date, scheduled completion, days late |
| Scheduling Advisor Output | Jobs identified as at risk of being late based on current schedule and capacity | Daily | Job number, customer, risk reason, days at risk |

### 8.2.7 Quality Reports

| Report Name | What It Shows | When to Run | Key Fields |
|---|---|---|---|
| Non-Conformance Report Log | All NCRs entered for a date range, with part, job, defect type, and disposition | Monthly | NCR number, date, part, job, defect type, disposition, cost |
| Inspection Records | Inspection results logged against jobs or parts, including pass/fail | On demand | Job number, part, inspection type, result, inspector, date |
| Vendor Quality Report | NCRs traceable to vendor-supplied material, summarized by vendor | Quarterly | Vendor, NCR count, defect types, cost of non-conformance |

---

## 8.3 Crystal Reports Integration — Complete Workflow

Crystal Reports is the gold standard for custom JobBOSS report development. It provides the deepest access to data, the most flexible formatting, and the most reliable performance when configured correctly. This section provides a complete, step-by-step procedure for creating, deploying, and maintaining custom Crystal Reports for JobBOSS.

### 8.3.1 Prerequisites

Before beginning Crystal Reports development, confirm the following are in place:

| Requirement | Details | Where to Obtain |
|---|---|---|
| Crystal Reports License | SAP Crystal Reports license for the version you intend to use | SAP / authorized resellers |
| SAP Crystal Reports Software | Installed on the developer workstation | SAP download portal |
| ODBC DSN | Configured Data Source Name pointing to JobBOSS SQL Server | Section 6.6 of this manual |
| SQL Server Credentials | Username and password with read access to the JobBOSS database | SQL Server administrator |
| JobBOSS Table Schema | Field and table reference documentation | ECI customer portal |
| JobBOSS Installation Path | Path to the Reporting directory on the application server | System administrator |

> **IMPORTANT:** The ODBC DSN must be a System DSN, not a User DSN, if Crystal Reports is running under a service account or if the report will be published to a server. A User DSN is only visible to the user who created it. A System DSN is visible to all users and services on that machine.

### 8.3.2 Step 1: Establish the ODBC Connection

Refer to Section 6.6 for complete ODBC setup instructions. When naming your DSN for Crystal Reports use:

- **Production reporting DSN name:** `JobBOSS_Prod`
- **Test/development DSN name:** `JobBOSS_Dev`

Using two separate DSNs allows you to test reports against a copy of the database without risk of impacting production data.

Verify the ODBC connection is working before proceeding:

1. Open ODBC Data Source Administrator (search "ODBC" in Windows Start menu — use the 32-bit version if JobBOSS is a 32-bit application).
2. Select the DSN you created.
3. Click **Test Connection**.
4. Connection Succeeded message must appear before proceeding.

> **32-BIT VS. 64-BIT WARNING:** JobBOSS is a 32-bit application on many installations. If Crystal Reports or ODBC connections fail to see the DSN, you may be using the 64-bit ODBC administrator when you need the 32-bit version. The 32-bit ODBC administrator is located at: `C:\Windows\SysWOW64\odbcad32.exe`. The 64-bit version is at `C:\Windows\System32\odbcad32.exe`. This naming convention is counterintuitive — SysWOW64 contains 32-bit components.

### 8.3.3 Step 2: Open Crystal Reports and Create a New Report

1. Launch SAP Crystal Reports.
2. From the Start page, click **Blank Report**, or navigate to **File → New → Blank Report**.
3. The **Database Expert** dialog opens automatically.

If the Database Expert does not open automatically:

1. Navigate to **Database → Database Expert**.
2. The dialog will open.

### 8.3.4 Step 3: Connect to the JobBOSS Data Source

**Method A: Standard Table Selection (for simple reports)**

1. In the Database Expert, expand **Create New Connection**.
2. Expand **ODBC (RDO)**.
3. Select your JobBOSS DSN from the list (e.g., `JobBOSS_Prod`).
4. Enter SQL Server credentials if prompted (or use Windows Authentication if configured).
5. Click **Finish**.
6. The available tables in the JobBOSS database will appear.

**Method B: SQL Command (REQUIRED for complex or high-performance reports)**

1. In the Database Expert, expand **Create New Connection**.
2. Expand **ODBC (RDO)**.
3. Select your JobBOSS DSN.
4. After connecting, instead of selecting tables from the list, right-click **Add Command** under the connection.
5. Type or paste your SQL query in the command window.
6. Click **OK**.

> **CRITICAL PERFORMANCE RULE — READ THIS BEFORE BUILDING ANY REPORT:**
>
> **The 4-Minute Report Problem:** The most common cause of unacceptably slow Crystal Reports in JobBOSS is the use of multiple Command objects or unfiltered table joins. When Crystal Reports joins multiple tables without a WHERE clause, it retrieves the entire contents of every table and performs the filtering in memory on the workstation. For a database with years of job history, this can mean Crystal Downloads hundreds of thousands of rows before the user sees any output.
>
> **The solution:** Use a single SQL Command object containing a complete query with proper WHERE clause filtering. Write the JOIN logic in SQL, not in Crystal's visual join editor. This causes the SQL Server database engine — which is optimized for this work — to do all filtering before sending data to Crystal Reports. A report that takes 4 minutes using multiple tables can be reduced to under 5 seconds using a single Command with proper SQL.
>
> **Rule:** If your report needs more than one table, write a SQL Command. Do not use the visual table drag-and-drop method for production reports.

### 8.3.5 Step 4: Select Tables and Configure Joins (If Using Standard Method)

If using Method A (standard table selection):

1. Double-click each table you need to move it to the **Selected Tables** list.
2. Crystal Reports will attempt to auto-join tables based on matching field names.
3. Carefully review each join line in the visual join editor:

   | Join Type | When to Use |
   |---|---|
   | Inner Join | Return only rows where matching records exist in both tables |
   | Left Outer Join | Return all rows from the left table, even if no match in right table |
   | Right Outer Join | Return all rows from the right table, even if no match in left table |

4. Use the table relationship reference in Section 6.4 of this manual to verify join fields are correct.
5. Common correct joins for JobBOSS reports:

   | Left Table | Left Field | Right Table | Right Field | Join Type |
   |---|---|---|---|---|
   | Job_Header | Customer_Code | Customer | Customer_Code | Left Outer |
   | Job_Header | Job_Number | Job_Operation | Job_Number | Left Outer |
   | Job_Operation | Work_Center | Work_Center | Work_Center | Left Outer |
   | Material_Log | Part_Number | Part_Master | Part_Number | Left Outer |
   | Purchase_Order | Vendor_Code | Vendor | Vendor_Code | Left Outer |

> **NOTE:** Table and field names listed above are representative. Verify exact names in the JobBOSS schema documentation available on the ECI customer portal. Table names and field names may vary by version.

### 8.3.6 Step 5: Design the Report Layout

Once the data source is configured, design the report in the Crystal Reports canvas.

**Report Sections:**

| Section | Purpose |
|---|---|
| Report Header | Title, company logo, run date/time — prints once at the start |
| Page Header | Column headings — prints at the top of every page |
| Group Header | Subtotal headings — prints at the start of each group |
| Detail | One row per data record |
| Group Footer | Subtotals per group — prints at the end of each group |
| Report Footer | Grand totals — prints once at the end |
| Page Footer | Page number, confidentiality notice — prints at the bottom of every page |

**Step-by-step for placing fields:**

1. In the **Field Explorer** panel (View → Field Explorer if not visible), expand **Database Fields**.
2. Drag fields from the Field Explorer onto the Detail section of the canvas.
3. Crystal will automatically create corresponding label objects in the Page Header section.
4. Resize columns by dragging field boundaries.

**Grouping data:**

1. Navigate to **Insert → Group**.
2. Select the field to group by (e.g., Customer_Code for a report grouped by customer).
3. Select sort direction (Ascending or Descending).
4. Group Header and Group Footer sections will be added automatically.

**Adding formulas:**

1. In the Field Explorer, right-click **Formula Fields → New**.
2. Name the formula (e.g., `Variance_Pct`).
3. Write the formula using Crystal Reports syntax:

   ```crystal
   // Example: Variance percentage formula
   If {Job_Header.Estimated_Cost} = 0 Then 0
   Else
   ({Job_Header.Actual_Cost} - {Job_Header.Estimated_Cost}) / {Job_Header.Estimated_Cost} * 100
   ```

4. Click **Save and Close**.
5. Drag the formula field onto the report canvas like any database field.

**Adding parameter fields for user input:**

1. In the Field Explorer, right-click **Parameter Fields → New**.
2. Define the parameter name, data type (Date, String, Number), and prompt text.
3. Reference the parameter in a record selection formula:

   ```crystal
   // Example: Date range parameter
   {Job_Header.Due_Date} >= {?Start_Date} And
   {Job_Header.Due_Date} <= {?End_Date}
   ```

4. Navigate to **Report → Selection Formulas → Record** and enter the selection formula.

> **UFL (User Function Library) WARNING:** Some standard JobBOSS Crystal Reports use functions from a proprietary User Function Library installed as part of the JobBOSS application. These functions are not part of Crystal Reports itself. If you see formula errors referencing functions that Crystal Reports does not recognize, those functions are UFL-based. You cannot replicate UFL functions in your custom reports without ECI involvement. Contact ECI support for documentation on available UFL functions.

### 8.3.7 Step 6: Save and Deploy the Report to JobBOSS

To make a custom Crystal Report accessible from within the JobBOSS application interface:

1. In Crystal Reports, navigate to **File → Report Properties** (or **File → Summary Info** depending on version).
2. In the **Title** field, enter the exact name the report should appear as in the JobBOSS menu. Note this name carefully — it must match exactly.
3. Save the .rpt file.
4. Navigate to the JobBOSS application installation directory on the server.
5. Locate the **Reporting** directory.
6. Inside Reporting, locate or create the **User-Defined** subdirectory.
7. Copy the .rpt file into: `[JobBOSS Install Path]\Reporting\User-Defined\`
8. In JobBOSS, navigate to **Reports → User Defined** (location may vary by version).
9. Your report name should appear in the list.

**Critical: Script Error Fix**

If you receive a script error when attempting to run a user-defined report from within JobBOSS, follow this procedure:

1. Navigate to the `[JobBOSS Install Path]\Reporting\` directory.
2. Locate the file named `UD_Report.htm`.
3. Navigate to the `[JobBOSS Install Path]\Reporting\User-Defined\` directory.
4. Verify that `UD_Report.htm` exists in BOTH directories.
5. Verify that the files in both locations are IDENTICAL (same date, same file size).
6. If missing from the User-Defined subdirectory, copy it from the Reporting directory.
7. Verify that the Report Name displayed in JobBOSS's User Defined list EXACTLY matches the **Title** field in the report's Crystal Reports Document Properties. Case sensitivity matters. A single character difference will cause the report to fail.

### 8.3.8 Step 7: Test and Maintain Reports After Upgrades

After any JobBOSS version upgrade, all custom Crystal Reports must be verified. This is not optional.

> **POST-UPGRADE WARNING:** Major JobBOSS version upgrades frequently modify the database schema. Tables may be renamed, fields may be added, removed, or renamed, and data types may change. Any custom Crystal Report that references fields by their exact string names — which is all of them — will break if those names change. Do not return to production use of any custom report without completing the verification steps below.

**Post-upgrade Crystal Reports verification procedure:**

1. Open each custom .rpt file in the Crystal Reports designer.
2. Navigate to **Database → Verify Database**.
3. Crystal Reports will compare the report's cached field list against the current database schema.
4. If fields have changed, Crystal Reports will prompt you with a list of broken field references.
5. Remap each broken field to its new name in the current schema.
6. Save the report.
7. Run the report against the database and verify the output is correct.
8. Only after all reports pass verification, mark them as production-ready.

**Recommended post-upgrade report test sequence:**

| Priority | Report to Test | What to Verify |
|---|---|---|
| 1 | Job Cost Report | Labor, material, and overhead costs appear correctly |
| 2 | Open Jobs Report | All open jobs display with correct status and due dates |
| 3 | Estimated vs. Actual | Estimated and actual figures both populate |
| 4 | AR Aging | Customer balances appear in correct aging buckets |
| 5 | Lot Tracking Report | Lot/heat number traceability chain is intact |
| 6 | All other custom reports | Data accuracy, formatting, and parameter filters work correctly |

---

## 8.4 Stimulsoft in JobBOSS²

JobBOSS² uses Stimulsoft as its embedded report designer. This section documents the behavior, limitations, and known issues with the Stimulsoft integration.

### 8.4.1 What Stimulsoft Provides

- A visual, drag-and-drop report designer accessible within the JobBOSS² web interface.
- Reports stored with the `.mrt` or `.cff` file extension depending on version.
- No additional software purchase required — Stimulsoft is included in the JobBOSS² subscription.
- Built-in reports for all standard JobBOSS modules are delivered in Stimulsoft format.

### 8.4.2 Stimulsoft Limitations in the JobBOSS² Context

| Limitation | Impact |
|---|---|
| Support path is ECI only | Cannot contact Stimulsoft directly; all issues must go through ECI support |
| Limited data source access | Only data sources ECI has pre-configured are available in the designer |
| Conditional logic bugs | Some embedded report templates contain known conditional expression errors |
| Variable comparison errors | Certain variable-based filters produce incorrect results in specific report templates |
| No direct SQL command access | Cannot write raw SQL in most embedded Stimulsoft report contexts |
| Learning curve | Stimulsoft's designer interface is not widely known; fewer freelance resources available |

### 8.4.3 Known Issues

- **Variable comparison error:** Some Stimulsoft report templates use variable comparisons that produce null or incorrect results when the compared fields contain empty strings vs. null values. Workaround: Contact ECI support for a corrected template.
- **Conditional visibility errors:** Conditional band visibility rules in some templates evaluate incorrectly when underlying data meets boundary conditions.
- **Print preview inconsistencies:** Page breaks in Stimulsoft preview occasionally differ from the printed or exported PDF output.

### 8.4.4 Recommended Strategy for JobBOSS² Reporting

Given the limitations above, the recommended reporting strategy for JobBOSS² users who need custom output is:

1. **Use built-in Stimulsoft reports** for standard operational reporting where they meet requirements.
2. **Use Crystal Reports** for custom reports requiring complex logic or specific formatting. Crystal Reports is still supported in JobBOSS² via ODBC connection to the underlying SQL Server.
3. **Use the JobBOSS² API with Excel or Power BI** for analytical and management reporting.
4. **Engage ECI or ReportingGuru.com** for custom Stimulsoft report development where the embedded designer is required.

---

## 8.5 Excel Reporting via ODBC (No Crystal Reports Required)

For power users who want direct data access without purchasing Crystal Reports, Excel can connect directly to the JobBOSS SQL Server database via ODBC and pull live data on demand.

### 8.5.1 Prerequisites

- ODBC DSN configured for the JobBOSS database (see Section 6.6).
- Microsoft Excel (any version with Get Data / Power Query).
- Basic SQL knowledge for writing queries.
- Read-only SQL Server login credentials.

### 8.5.2 Step-by-Step: Connect Excel to JobBOSS via ODBC

1. Open Microsoft Excel.
2. Navigate to the **Data** tab on the ribbon.
3. Click **Get Data → From Other Sources → From ODBC**.
4. In the **From ODBC** dialog, select the JobBOSS DSN from the dropdown.
5. If prompted for credentials, enter the SQL Server username and password. Select **Database** as the credential type (not Windows).
6. Click **Connect**.
7. The Navigator dialog will appear, showing available tables.

**Option A: Load a single table**
- Browse to the desired table.
- Click **Load** to import the full table, or **Transform Data** to open Power Query Editor for filtering.

**Option B: Write a SQL query for precise data**
- In the **From ODBC** dialog, before connecting, expand **Advanced Options**.
- In the **SQL statement** field, enter your query:

   ```sql
   SELECT
       jh.Job_Number,
       jh.Customer_Code,
       c.Customer_Name,
       jh.Part_Number,
       jh.Order_Quantity,
       jh.Due_Date,
       jh.Status,
       jh.Estimated_Cost,
       jh.Actual_Cost,
       (jh.Actual_Cost - jh.Estimated_Cost) AS Cost_Variance
   FROM Job_Header jh
   LEFT JOIN Customer c ON jh.Customer_Code = c.Customer_Code
   WHERE jh.Status NOT IN ('Closed', 'Shipped')
   ORDER BY jh.Due_Date ASC
   ```

- Click **OK** to run the query and load results into Excel.

8. Data loads into the Excel worksheet as a table.
9. Build PivotTables, charts, and formulas on this data as needed.
10. To refresh with current data: right-click the table → **Refresh**, or use **Data → Refresh All**.

### 8.5.3 Saving Queries for Reuse

Excel saves the ODBC connection and query within the workbook. To make this a reusable reporting tool:

1. After loading data, navigate to **Data → Queries and Connections**.
2. Right-click the query → **Properties**.
3. Set the **Refresh control** options:
   - Check **Refresh data when opening the file** for automatic refresh on open.
   - Set a **Refresh every X minutes** interval if running as a live dashboard.
4. Protect the worksheet if sharing with users who should not modify the query.

### 8.5.4 Limitations

| Limitation | Details |
|---|---|
| SQL knowledge required | You must write valid T-SQL to query effectively |
| No parameter prompts | Cannot prompt for date range at run time without VBA macros |
| Not suitable for non-technical users | Requires setup and maintenance by a power user |
| Data is read-only | Cannot write data back to JobBOSS from Excel |
| Performance | Large queries may be slow without proper WHERE clause filtering |

---

## 8.6 Power BI Integration

### 8.6.1 JobBOSS² API Overview

JobBOSS² exposes a REST API that Power BI Desktop can connect to for reporting and analytics. This is the cloud-native reporting path for JobBOSS² users who want dashboards and visual analytics.

| API Property | Details |
|---|---|
| API Type | REST API (JSON responses) |
| Authentication Method | OAuth 2.0 with client_id and client_secret |
| Endpoint Base URL | Provided by ECI upon request; varies by tenant |
| Documentation | Available on the ECI customer portal |

### 8.6.2 Connecting Power BI Desktop to JobBOSS²

1. Obtain your `client_id` and `client_secret` from ECI support or the JobBOSS² admin portal.
2. Open **Power BI Desktop**.
3. Click **Get Data → Web**.
4. Enter the API endpoint URL.
5. Select **Advanced** and configure the authentication header:
   - Authorization: `Bearer [token]`
6. If the connection fails at this step, use the **Web API** connector with OAuth2 authentication type instead.

> **KNOWN ISSUE:** Some Power BI Desktop users report inability to connect to the JobBOSS² API even with valid credentials. This is a known integration issue. If connection fails:
> 1. Verify your client_id and client_secret are current and have not expired.
> 2. Confirm the API endpoint URL format with ECI support — it may have changed in recent versions.
> 3. Request ECI's current Power BI connection guide from the customer portal.
> 4. As an alternative, use the ODBC connection to SQL Server if you have access to the underlying database server.

### 8.6.3 Alternative: Power BI via ODBC

For on-premise JobBOSS or JobBOSS² with database access:

1. In Power BI Desktop, click **Get Data → ODBC**.
2. Select the JobBOSS DSN.
3. Enter credentials.
4. Select tables or write a SQL query.
5. Load data and build visuals.

This method bypasses the API entirely and provides full SQL flexibility.

### 8.6.4 Recommended Power BI Datasets for Die-Mension

| Dataset | Source Tables | Refresh Frequency |
|---|---|---|
| Open Jobs Status | Job_Header, Customer | Daily |
| Jobs Behind Schedule | Job_Header, Job_Operation | Daily |
| Monthly Revenue vs. Cost | Job_Header, Invoice_Header | Weekly |
| AR Aging | AR_Aging or AR_Open | Weekly |
| Vendor On-Time Delivery | Purchase_Order, Vendor | Monthly |
| Inventory Value Trend | Part_Master, Inventory | Monthly |
| Quote Win Rate | Quote_Header, Customer | Monthly |

---

## 8.7 KPI Dashboard Configuration

### 8.7.1 JobBOSS Built-In Dashboard

The JobBOSS application includes a configurable home screen dashboard with KPI tiles. Each user can configure their own dashboard view.

**To configure the dashboard:**

1. Log into JobBOSS.
2. Navigate to the home screen / dashboard view.
3. Click **Customize** or the gear icon (location varies by version).
4. Select from available KPI widgets.
5. Arrange widgets by drag-and-drop.
6. Set threshold alerts where available (e.g., alert when AR over 60 days exceeds $5,000).
7. Save the dashboard layout.

### 8.7.2 Recommended KPI Tiles for Die-Mension

| KPI | Target / Alert Threshold | Responsible Party |
|---|---|---|
| Jobs Behind Schedule | Alert when count > 0 | Karen |
| Open Quotes Awaiting Follow-Up | Alert when count > 5 and age > 14 days | Karen |
| Inventory Value | Monitor monthly for trend | Karen |
| Overdue Purchase Orders | Alert when count > 0 | Karen |
| AR Over 60 Days | Alert when balance > $5,000 | Karen |
| On-Time Delivery % (rolling 90 days) | Alert when below 95% | Karen |
| Open Jobs Count | Informational | Karen |
| Jobs Shipping This Week | Informational | Shop floor |

### 8.7.3 KnowledgeSync Integration for Automated Alerts

**KnowledgeSync** is a separately licensed add-on for JobBOSS that provides automated alert and notification capabilities beyond the built-in dashboard.

| Feature | Details |
|---|---|
| Trigger Types | Database query results, schedule-based, event-based |
| Actions | Email, SMS, file creation, workflow automation |
| Use Cases | Late job alerts, overdue PO notifications, low inventory alerts, AR past-due notices |
| License | Separately purchased from ECI; not included in base JobBOSS license |
| Configuration | Query-driven; requires SQL knowledge to set up custom alerts |

> **DIE-MENSION RECOMMENDATION:** KnowledgeSync is worthwhile for Die-Mension once the base system is stable. The most valuable alert would be an automatic email to Karen when any job's projected completion date exceeds its due date — enabling proactive customer communication before the customer calls asking where their parts are.

---

## 8.8 Reporting Best Practices for Die-Mension

The following reports should be configured and running before Die-Mension goes live. These are the minimum viable reporting set for a precision tool and die shop.

### 8.8.1 Priority Report Setup Checklist

| Priority | Report | Frequency | Who Needs It | Why |
|---|---|---|---|---|
| 1 | Open Jobs with Due Dates | Daily | Karen | Know what's coming up and what's at risk |
| 2 | Jobs Behind Schedule | Daily | Karen | Required for proactive customer communication |
| 3 | Open PO Report | Daily | Karen | Know what's on order and when it arrives |
| 4 | Inventory Reorder Report | Weekly | Karen | Prevent material stockouts that delay jobs |
| 5 | Job Profitability Report | Weekly | Karen | Are we making money on jobs? |
| 6 | Estimated vs. Actual Report | Monthly | Karen | Is our estimating accurate? |
| 7 | AR Aging Report | Weekly | Karen | Are customers paying? Who needs a call? |
| 8 | Lot Tracking Report | On demand | Karen | Answer customer heat number audit trail questions |
| 9 | Quote Win/Loss Report | Monthly | Karen | Are we winning quotes? What's our hit rate? |
| 10 | Vendor Performance Report | Monthly | Karen | Are vendors delivering on time? |

### 8.8.2 Report Distribution Recommendations

At Die-Mension's size, formal report distribution infrastructure is not necessary. Recommended approach:

- **Daily:** Karen reviews the Open Jobs and Jobs Behind Schedule report each morning before the shop floor opens.
- **Weekly:** AR Aging, Open PO, and Job Profitability reports reviewed every Monday morning.
- **Monthly:** Estimated vs. Actual, Vendor Performance, and Quote Win/Loss reviewed at the end of each month for process improvement discussions.
- **On demand:** Lot Tracking run as needed when customers request material traceability.

### 8.8.3 Report Naming Conventions

Use consistent, descriptive names for all reports. Recommended naming convention:

`[Module]-[Description]-[Frequency or Scope]`

**Examples:**
- `JOBS-OpenJobsDueDates-Daily`
- `JOBS-EstimatedVsActual-Monthly`
- `AR-AgingReport-Weekly`
- `INV-ReorderReport-Weekly`
- `QUALITY-LotTracking-OnDemand`

Consistent naming makes reports easy to find in the User-Defined section and easy to identify in the Reporting directory on the server.

---

---

# SECTION 9: SYSTEM ADMINISTRATION AND CONFIGURATION

## 9.1 Overview: System Administrator Role

The JobBOSS system administrator is the person responsible for the health, accuracy, and availability of the system. At a small shop like Die-Mension, this role will be held by Karen. This does not require IT expertise — it requires ownership, attention to detail, and a commitment to keeping the system well-maintained.

> **MOST CRITICAL IMPLEMENTATION SUCCESS FACTOR:** The single most common reason JobBOSS implementations fail at small shops is the absence of a dedicated system owner. When no one owns the system, master data goes unmaintained, user questions go unanswered, performance issues are not addressed, and the shop reverts to spreadsheets and whiteboards within six months. At Die-Mension, Karen is designated as the system owner. This is not optional.

### 9.1.1 System Administrator Responsibilities Summary

| Responsibility Area | Tasks | Frequency |
|---|---|---|
| Master Data (Table Maintenance) | Create and update work centers, operations, employees, bin locations, UOM, customer/vendor classes | As needed; review quarterly |
| User Management | Create accounts, reset passwords, adjust permissions, deactivate former employees | As needed |
| Database Backups | Verify backup jobs completed; test restores | Daily verification; monthly restore test |
| Software Updates | Apply minor updates; manage major version upgrades | Per ECI release schedule |
| ODBC Connections | Verify DSN connections after upgrades or server changes | After every upgrade |
| Integration Maintenance | Monitor QuickBooks sync, data collection terminals, API connections | Weekly |
| Reporting | Add/update Crystal Reports, maintain user-defined report directory | As needed |
| Performance Monitoring | Monitor slow queries, index fragmentation, database growth | Monthly |

### 9.1.2 System Administrator Tools Required

| Tool | Purpose | Where to Obtain |
|---|---|---|
| SQL Server Management Studio (SSMS) | Database maintenance, backup verification, query analysis | Free from Microsoft |
| ODBC Data Source Administrator | Configure and test ODBC DSNs | Built into Windows |
| SAP Crystal Reports | Custom report development | SAP (licensed) |
| ECI Customer Portal login | Software downloads, release notes, support tickets | ECI onboarding |
| Windows Server access (on-premise) | Server-level administration | IT or hosting provider |
| Remote Desktop / VPN | Remote administration access | IT |

---

## 9.2 Table Maintenance: Master Data Configuration

Table Maintenance is the primary configuration area for all master data in JobBOSS. It is accessible from: **Main Menu → Table Maintenance** or **Administration → Table Maintenance**, depending on the version and menu layout.

> **FOUNDATIONAL RULE:** All scheduling, costing, and routing in JobBOSS is built on top of the master data configured in Table Maintenance. If work centers have wrong capacity values, the schedule is wrong. If operations have wrong standard times, estimates are wrong. If employees have wrong labor rates, job costs are wrong. Invest the necessary time to configure Table Maintenance correctly before going live. Fixing master data after jobs have been entered is significantly more time-consuming than configuring it correctly the first time.

### 9.2.1 Work Centers

Work centers represent the physical machines or work areas on the shop floor. Every routing operation on every job must be assigned to a work center. Work center capacity settings drive all scheduling calculations.

**Required fields for each work center:**

| Field | Description | Example Value |
|---|---|---|
| Work Center Code | Short identifier, no spaces, uppercase | `WIRE-EDM` |
| Description | Full descriptive name | `Wire EDM - Fanuc Alpha-C` |
| Capacity (hours/day) | Available productive hours per day per machine | `7.5` |
| Number of Machines | How many identical machines at this work center | `2` |
| Labor Rate ($/hr) | Fully burdened labor cost per hour | `85.00` |
| Overhead Rate ($/hr) | Machine overhead per hour, if applying separately | `45.00` |
| Machine Rate ($/hr) | Machine cost per hour | `35.00` |
| Department | Assigned department for reporting | `MACHINING` |
| Number of Shifts | Shifts per day this work center operates | `1` |

**Step-by-step procedure to create a work center:**

1. Navigate to **Table Maintenance → Work Centers**.
2. Click **New** (or the + button, depending on version).
3. Enter the **Work Center Code**. Use all uppercase. No spaces — use hyphens. Maximum typically 8–10 characters. Examples: `MILL-1`, `GRIND`, `EDM-WIRE`, `BENCH`.
4. Enter the **Description** — the full name that will appear in reports and on travelers.
5. Set **Hours Per Shift** to the productive hours available (typically 7.5 for an 8-hour shift with 30 minutes downtime).
6. Set **Number of Shifts** per day.
7. Set **Number of Machines** at this work center (identical machines can share one work center code or be given separate codes — separate codes give more precise scheduling visibility).
8. Enter the **Labor Rate** in dollars per hour. This should be the fully burdened shop rate, not just the wage.
9. Enter the **Overhead Rate** if using machine overhead as a separate cost component.
10. Assign to a **Department** if using departments for reporting.
11. Click **Save**.

**Work Centers to Create for Die-Mension:**

| Work Center Code | Description | Notes |
|---|---|---|
| `SETUP` | Setup and Programming | CNC programming, machine setup |
| `MILL-VMC` | Vertical Machining Center | CNC milling |
| `MILL-HMC` | Horizontal Machining Center | If applicable |
| `TURN-CNC` | CNC Lathe / Turning Center | CNC turning |
| `TURN-MAN` | Manual Lathe | Manual turning operations |
| `GRIND-SURF` | Surface Grinder | Surface grinding |
| `GRIND-CYL` | Cylindrical Grinder | OD/ID cylindrical grinding |
| `EDM-WIRE` | Wire EDM | Wire electro-discharge machining |
| `EDM-SINK` | Sinker EDM | Ram/sinker EDM |
| `BENCH` | Bench Work / Hand Fitting | Fitting, assembly, deburring |
| `HEAT-TREAT` | Heat Treat (Outside Process) | If outsourced, mark as outside process |
| `INSPECT` | Inspection / QC | CMM, optical comparator, manual |
| `SHIP` | Shipping and Receiving | Final pack and ship |

> **OUTSIDE PROCESSES:** If Heat Treat, Plating, or other processes are outsourced, you can create a work center for them and mark it as an **Outside Process** type. This allows proper cost routing while identifying that the operation is performed by a vendor rather than in-house.

### 9.2.2 Operations

Operations are the reusable, standardized steps that are added to job routings. Each operation is associated with a work center and can include standard setup and run times to speed up estimating.

**Required fields for each operation:**

| Field | Description | Example |
|---|---|---|
| Operation Code | Short identifier matching the work center it applies to | `EDM-WIRE-PROG` |
| Description | Full description of the operation | `Wire EDM Programming` |
| Work Center | The work center where this operation is performed | `EDM-WIRE` |
| Standard Setup Time | Default setup hours per run (can be overridden on job) | `1.5` |
| Standard Run Time | Default run time per piece in hours (can be overridden on job) | `0.25` |

**Step-by-step to create an operation:**

1. Navigate to **Table Maintenance → Operations**.
2. Click **New**.
3. Enter the **Operation Code**. Recommend a naming convention that reflects the work center and action: `MILL-ROUGH`, `MILL-FINISH`, `GRIND-FLAT`, `EDM-WIRE-CUT`, `INSPECT-CMM`, `INSPECT-VISUAL`.
4. Enter the **Description**.
5. Select the **Work Center** this operation is performed at.
6. Enter **Standard Setup Time** in hours, if known. This populates as a default when the operation is added to a routing but can be overridden per job.
7. Enter **Standard Run Time** per piece in hours, if known.
8. Click **Save**.

**Recommended Operations for Die-Mension:**

| Operation Code | Description | Work Center |
|---|---|---|
| `MILL-SETUP` | CNC Mill Setup | `MILL-VMC` |
| `MILL-ROUGH` | CNC Milling - Roughing | `MILL-VMC` |
| `MILL-FINISH` | CNC Milling - Finishing | `MILL-VMC` |
| `TURN-SETUP` | CNC Turning Setup | `TURN-CNC` |
| `TURN-RUN` | CNC Turning - Run | `TURN-CNC` |
| `GRIND-FLAT` | Surface Grinding - Flat | `GRIND-SURF` |
| `GRIND-OD` | Cylindrical Grinding - OD | `GRIND-CYL` |
| `EDM-PROG` | Wire EDM Programming | `EDM-WIRE` |
| `EDM-WIRE-CUT` | Wire EDM - Cut | `EDM-WIRE` |
| `EDM-SINK-RUN` | Sinker EDM - Run | `EDM-SINK` |
| `BENCH-FIT` | Bench Fitting / Assembly | `BENCH` |
| `HEAT-TREAT-OUT` | Heat Treat (Outside) | `HEAT-TREAT` |
| `INSPECT-CMM` | CMM Inspection | `INSPECT` |
| `INSPECT-VISUAL` | Visual Inspection | `INSPECT` |
| `SHIP-PACK` | Pack and Ship | `SHIP` |

### 9.2.3 Employees

Every person who clocks in or out of jobs in JobBOSS — whether using data collection terminals or manually — must have an Employee record.

**Required fields per employee:**

| Field | Description | Notes |
|---|---|---|
| Employee ID | Numeric ID used at data collection terminals | Keep short: 4–5 digits |
| First Name | Employee first name | |
| Last Name | Employee last name | |
| Labor Rate | Fully burdened hourly cost rate in $/hr | NOT their wage — their cost to the shop |
| Default Work Center | The work center they primarily work at | Can be overridden when clocking in |
| Department | Department assignment for reporting | |
| Pay Type | Hourly, Salary, or Other | |
| Active | Active/Inactive status | Set Inactive for former employees — do not delete |

**Step-by-step to create an employee:**

1. Navigate to **Table Maintenance → Employees**.
2. Click **New**.
3. Enter the **Employee ID**. This is what the employee types at the data collection terminal. Use a simple numeric code they can remember.
4. Enter **First Name** and **Last Name**.
5. Enter the **Labor Rate**. This is the shop's cost per hour for this employee — typically wage plus burden (taxes, benefits, insurance). Do not enter the raw wage if you want accurate job costing. Consult with accounting to determine the correct fully burdened rate.
6. Select the **Default Work Center**.
7. Assign to a **Department** if applicable.
8. Set **Pay Type**.
9. Verify **Active** status is checked.
10. Click **Save**.

> **PRIVACY NOTE:** Labor rates in JobBOSS are visible to any user with access to Employee records or job cost reports. Control access to Employee records using role-based permissions (Section 9.3). Karen should be the only user with access to Employee labor rates.

**Inactive vs. Delete:**
- When an employee leaves, set their record to **Inactive**. Do not delete the record.
- Deleting an employee record will cause historical job cost records that reference that employee to lose their labor data linkage.
- Inactive employees do not appear in dropdowns but their historical data remains intact.

### 9.2.4 Bin Locations

Bin locations are physical storage locations in the facility. Every part and material stored in inventory must be assignable to a bin location. Proper bin location setup is a prerequisite for material control and cycle counting.

See Section 3 of this manual for the complete bin location setup procedure. Summary steps:

1. Navigate to **Table Maintenance → Bin Locations**.
2. Click **New** for each location.
3. Enter the **Location Code** (e.g., `RACK-A-1`, `SHELF-B-3`, `FLOOR-D2`).
4. Enter the **Description** (e.g., "Rack A, Row 1, Top Shelf").
5. Check **Active**.
6. Click **Save**.

**Bin Location Naming Convention for Die-Mension:**

| Format | Example | Description |
|---|---|---|
| `RACK-[Row]-[Shelf]` | `RACK-A-1` | Shelving rack, Row A, Shelf 1 |
| `SHELF-[ID]-[Position]` | `SHELF-B-3` | Free-standing shelf, Bay B, Position 3 |
| `FLOOR-[Zone]` | `FLOOR-D2` | Floor storage, Zone D2 |
| `CABINET-[ID]` | `CABINET-TOOLING` | Locked tooling cabinet |
| `RECEIVING` | `RECEIVING` | Receiving dock holding area |
| `QC-HOLD` | `QC-HOLD` | Quality hold area |
| `SHIP-STAGING` | `SHIP-STAGING` | Ready-to-ship staging area |

### 9.2.5 Customer Classes and Vendor Classes

Customer and vendor classes allow grouping for reporting and filtering.

**Customer Class Setup:**

1. Navigate to **Table Maintenance → Customer Classes** (or this may be within Customer master settings).
2. Create classes appropriate for Die-Mension's customer base:

| Class Code | Description |
|---|---|
| `AUTOMOTIVE` | Automotive industry customers |
| `AEROSPACE` | Aerospace and defense customers |
| `MOLDMAKER` | Mold and die industry customers |
| `MEDICAL` | Medical device customers |
| `GENERAL` | General machining customers |
| `PROTOTYPE` | Prototype / one-off customers |

**Vendor Class Setup:**

1. Navigate to **Table Maintenance → Vendor Classes**.
2. Create classes for Die-Mension's vendor base:

| Class Code | Description |
|---|---|
| `STEEL-SUPPLIER` | Raw material steel suppliers |
| `TOOLING` | Cutting tools and tooling suppliers |
| `SUBCONTRACT` | Subcontract / outside process vendors |
| `HEAT-TREAT` | Heat treat vendors |
| `PLATING` | Plating and coating vendors |
| `SERVICES` | Utilities and services |
| `EQUIPMENT` | Machine and equipment vendors |

### 9.2.6 Units of Measure

All UOM codes must be created in Table Maintenance before they can be used on Part Master records. Create these before entering any parts.

**Recommended UOM codes for Die-Mension:**

| Code | Description | Typical Use |
|---|---|---|
| `EA` | Each | Individual piece parts, tooling |
| `LBS` | Pounds | Steel bar stock, coil stock |
| `FT` | Feet | Linear material sold by the foot |
| `IN` | Inches | Linear material sold by the inch |
| `KG` | Kilograms | Material purchased internationally |
| `SQFT` | Square Feet | Plate material |
| `LOT` | Lot | Miscellaneous, one-time purchases |

> **COIL STOCK NOTE:** For steel coil stock and bar stock, `LBS` is the recommended UOM because steel is typically priced and purchased by weight. Tracking by LBS also makes receiving and inventory valuation more accurate when lengths vary.

---

## 9.3 User Account Administration

### 9.3.1 Known Limitations in JobBOSS User Administration

Before investing significant time in user permission configuration, be aware of these documented limitations:

| Issue ID | Description | Impact |
|---|---|---|
| JBCORE-I-1023 | Limited permission granularity — cannot restrict access to specific fields within a module | Users who can see a module can see all fields in that module |
| JBCORE-I-1068 | Missing true role-based access controls — permissions are largely all-or-nothing per module | Difficult to create a "read-only" user for specific modules |
| JBCORE-I-1100 | Cannot restrict access to specific customers or specific jobs — access is all or nothing | Confidential pricing visible to any user with job access |

These are known issues logged in ECI's Ideas Portal. They may be addressed in future releases. Design your permission structure around these limitations rather than fighting them.

### 9.3.2 Step-by-Step: Creating a New User

1. Navigate to **Administration → User Management** (may appear as **System → Users** in some versions).
2. Click **New User** or the **+** button.
3. Enter the **Username**. Recommended convention: `firstname.lastname` (e.g., `karen.mitchell`).
4. Enter a temporary **Password**. The user should change it on first login.
5. Enter the user's **Email Address** (required for password reset and KnowledgeSync notifications).
6. Select the **Permission Level** or **Role**:

   | Role Level | Appropriate For | Access |
   |---|---|---|
   | Administrator | Karen (system owner only) | Full access to all modules and Table Maintenance |
   | Full User | Estimators, office staff | Access to Jobs, Quoting, Purchasing, Reporting; no Table Maintenance |
   | Data Entry | Shop floor lead who enters job completions manually | Limited to job data entry |
   | Read Only | Observers, temporary staff | Can view data but cannot enter or modify |
   | Shop Floor (Terminal) | Data collection terminal users | Clock in/out only |

7. Associate the user with an **Employee Record** if they are a shop floor employee. This links their User account to their Employee ID for data collection.
8. Set **Active** status to checked.
9. Click **Save**.
10. Notify the user of their credentials and have them log in and change their password.

### 9.3.3 Best Practices for User Account Management

**Do not share accounts.** Every person who accesses JobBOSS must have their own account. Individual accountability — knowing who entered which record and when — requires individual logins. Shared accounts make audit trails meaningless.

**Deactivate, do not delete, departing employees.** When a user leaves:
1. Navigate to their User account.
2. Set **Active** to unchecked (deactivated).
3. Do not delete the account — historical records reference the username.

**Password policy:**
- Require passwords to be changed on first login.
- Recommend a minimum 10-character password.
- Do not store passwords in plaintext anywhere.

**Periodic access review:**
- Quarterly, review the active user list.
- Confirm each active user still requires access.
- Deactivate any accounts for users who have left or changed roles.

### 9.3.4 Recommended User Accounts for Die-Mension

| Username | Role | Access Level | Notes |
|---|---|---|---|
| `karen.mitchell` | System Administrator | Administrator | Full access; only admin account |
| `shop.lead` | Shop Floor Lead | Full User | Job entry, routing updates, no accounting |
| `terminal.user` | Data Collection | Shop Floor | Clock in/out only; used on shop floor terminals |

> **DIE-MENSION NOTE:** At Die-Mension's current size, two to three user accounts will likely cover all operational needs. Resist the temptation to create accounts for every scenario until they are actually needed. Unused accounts represent a security risk.

---

## 9.4 Chart of Accounts Configuration

The Chart of Accounts (COA) must be fully configured before the General Ledger module is activated. If JobBOSS is being integrated with QuickBooks, the COA in JobBOSS must mirror the QuickBooks COA exactly before the integration is enabled.

### 9.4.1 Step-by-Step: Accessing Chart of Accounts

1. Navigate to **Accounting → Chart of Accounts** (or **GL → Chart of Accounts**, depending on version).
2. The COA screen shows all configured accounts in account number order.
3. To add an account, click **New**.
4. To edit, click on the account number.

### 9.4.2 Minimum Required Account Structure for a Small Manufacturer

The following accounts represent a minimum viable COA for Die-Mension. Account numbers follow a typical small business structure:

**Income Accounts (4000s):**

| Account Number | Account Name | Description |
|---|---|---|
| 4000 | Sales Revenue | Primary revenue from job invoicing |
| 4010 | Scrap Sales | Revenue from material scrap sales |
| 4090 | Other Income | Miscellaneous non-job revenue |

**Cost of Goods Sold Accounts (5000s):**

| Account Number | Account Name | Description |
|---|---|---|
| 5000 | Direct Labor | Labor cost directly charged to jobs |
| 5010 | Direct Materials | Material cost directly charged to jobs |
| 5020 | Outside Processing | Subcontract and vendor processing costs |
| 5030 | Manufacturing Overhead | Applied manufacturing overhead |
| 5040 | Scrap and Rework | Cost of internal scrap and rework |

**Operating Expense Accounts (6000s):**

| Account Number | Account Name | Description |
|---|---|---|
| 6000 | Salaries and Wages - SG&A | Non-production staff salaries |
| 6010 | Payroll Taxes | Employer payroll tax expense |
| 6020 | Employee Benefits | Health insurance, retirement, etc. |
| 6030 | Utilities | Electric, gas, water for the facility |
| 6040 | Rent / Lease | Facility rent or lease payments |
| 6050 | Equipment Depreciation | Depreciation on shop equipment |
| 6060 | Tooling and Supplies | Consumable tooling and shop supplies |
| 6070 | Insurance | Business insurance premiums |
| 6080 | Professional Fees | Accounting, legal, consulting |
| 6090 | Advertising and Marketing | Marketing expenses |
| 6100 | Vehicle Expense | Company vehicle costs |

**Asset Accounts (1000s):**

| Account Number | Account Name | Description |
|---|---|---|
| 1000 | Cash - Checking | Primary operating checking account |
| 1010 | Cash - Savings | Savings or reserve account |
| 1100 | Accounts Receivable | Customer balances owed |
| 1200 | Raw Material Inventory | Value of raw materials on hand |
| 1210 | WIP Inventory | Work-in-process inventory value |
| 1220 | Finished Goods Inventory | Finished goods awaiting shipment |
| 1500 | Equipment | Gross value of shop equipment |
| 1510 | Accumulated Depreciation - Equipment | Contra-asset for equipment depreciation |

**Liability Accounts (2000s):**

| Account Number | Account Name | Description |
|---|---|---|
| 2000 | Accounts Payable | Vendor balances owed |
| 2100 | Accrued Payroll | Wages earned but not yet paid |
| 2200 | Sales Tax Payable | Collected sales tax awaiting remittance |
| 2300 | Equipment Loans | Long-term equipment financing |
| 2400 | Other Current Liabilities | Miscellaneous short-term liabilities |

**Equity Accounts (3000s):**

| Account Number | Account Name | Description |
|---|---|---|
| 3000 | Owner's Equity | Owner's invested capital |
| 3900 | Retained Earnings | Cumulative prior-year net income |

### 9.4.3 QuickBooks Integration Account Matching Requirement

> **CRITICAL — QuickBooks Integration:** If Die-Mension uses QuickBooks as the primary accounting system, the following rules apply without exception:
>
> 1. Every account name in JobBOSS must match a QuickBooks account name **exactly** — same capitalization, same spacing, same punctuation.
> 2. QuickBooks Online does not permit duplicate account names. If a name exists in QBO, it cannot be used again in a sub-account with the same name.
> 3. Do not activate the QuickBooks integration in JobBOSS until the COA match has been verified for every account.
> 4. Set up JobBOSS GL accounts to match QuickBooks FIRST, before any transactions are posted. Remapping accounts after transactions have posted is extremely time-consuming and may require ECI support.

---

## 9.5 SQL Server Maintenance Plan

### 9.5.1 Why Database Maintenance Is Required

SQL Server databases accumulate index fragmentation over time as records are inserted, updated, and deleted. Fragmented indexes cause the SQL Server query engine to read far more data pages than necessary to answer any given query. The performance impact is cumulative and progressive:

| Time Since Last Maintenance | Typical Performance Impact |
|---|---|
| Day 1 (fresh installation) | Baseline — queries run at full speed |
| 6 months, no maintenance | 10–20% slower for heavily updated tables |
| 1 year, no maintenance | 30–50% slower; noticeable in daily use |
| 2+ years, no maintenance | Queries that took 1 second may take 30+ seconds |

The solution is a scheduled SQL Server maintenance plan that runs automatically during off-hours.

### 9.5.2 Setting Up a Maintenance Plan in SQL Server Management Studio

**Prerequisites:**
- SQL Server Management Studio (SSMS) installed (free from Microsoft).
- Login credentials for the SQL Server instance with sysadmin privileges.
- SQL Server Agent service must be running (this is what runs scheduled maintenance jobs).

**Procedure:**

1. Open **SQL Server Management Studio (SSMS)**.
2. Connect to the SQL Server instance hosting the JobBOSS database.
3. In the Object Explorer panel, expand **Management**.
4. Right-click **Maintenance Plans**.
5. Select **Maintenance Plan Wizard**.
6. Click **Next** on the welcome screen.
7. Name the plan: `JobBOSS Weekly Maintenance`.
8. Select **Separate schedules for each task** to allow different schedules for different tasks.
9. Click **Next**.

**Select Maintenance Tasks:**
Check the following tasks:
- [x] **Check Database Integrity**
- [x] **Rebuild Index**
- [x] **Update Statistics**
- [x] **Back Up Database (Full)**

10. Click **Next**.

**Configure Rebuild Index Task:**

| Setting | Value |
|---|---|
| Database(s) | Select the JobBOSS database (e.g., `JobBoss32` or your database name) |
| Object | Tables and Views |
| Free space per page | 10% (default) |
| Sort results in tempdb | Checked (recommended for large databases) |
| Keep index online | Unchecked (requires Enterprise edition) |

Schedule: Every Sunday at 11:00 PM.

**Configure Update Statistics Task:**

| Setting | Value |
|---|---|
| Database(s) | JobBOSS database |
| Object | All existing tables and views |
| Update | All existing statistics |

Schedule: Every Sunday at 11:30 PM (after index rebuild completes).

**Configure Database Backup Task:**

| Setting | Value |
|---|---|
| Database(s) | JobBOSS database |
| Backup type | Full |
| Backup to | Disk |
| Backup file location | Local path AND mapped network path for off-site copy |
| Verify backup integrity | Checked |
| Schedule | Daily at 2:00 AM |

11. Click **Next** through remaining screens.
12. Click **Finish** to create the maintenance plan.
13. In Object Explorer, expand **SQL Server Agent → Jobs** to confirm the maintenance job was created.
14. Right-click the new job and select **Start Job at Step** to test it immediately.
15. Verify the job completes successfully before relying on the schedule.

### 9.5.3 Backup Strategy

A backup that has never been tested is not a backup. It is a file of unknown status.

**Minimum backup requirements for Die-Mension:**

| Backup Type | Frequency | Retention | Storage Location |
|---|---|---|---|
| Full SQL Server backup | Daily (automated) | 30 days minimum | Local server drive |
| Full SQL Server backup | Daily (automated copy) | 30 days | Network-attached storage or cloud |
| Manual backup before upgrades | Before every upgrade | Keep indefinitely | Separate labeled folder |

**Monthly backup restore test procedure:**

1. Identify the most recent backup file.
2. On a non-production machine or SQL Server instance, attempt to restore the backup.
3. Verify the database restores completely without errors.
4. Open SSMS and run a basic query against the restored database to confirm data integrity:

   ```sql
   SELECT COUNT(*) FROM Job_Header;
   SELECT COUNT(*) FROM Customer;
   SELECT TOP 10 * FROM Job_Header ORDER BY Job_Number DESC;
   ```

5. Document the test date and result.
6. If the restore fails, investigate immediately — do not wait until a disaster.

> **CLOUD-HOSTED (JobBOSS²) BACKUP NOTE:** ECI manages server infrastructure and performs backups at the platform level for JobBOSS² cloud customers. However, ECI's SLA does not protect against user error (such as accidentally deleting records). Karen should export key datasets to Excel monthly as a personal data safety net: Customer list, Open jobs list, Inventory snapshot, Open PO list. Store these exports in a folder on a local machine or cloud drive (OneDrive, Google Drive).

### 9.5.4 SQL Server Version Compatibility

> **WARNING:** Never upgrade the SQL Server version without first verifying compatibility with the installed version of JobBOSS. Incompatible combinations can prevent JobBOSS from connecting to the database at all, resulting in a complete system outage.

| SQL Server Version | JobBOSS Compatibility Notes |
|---|---|
| SQL Server 2017 | Confirmed compatible with JobBOSS 12.x |
| SQL Server 2019 | Check ECI customer portal for specific version compatibility |
| SQL Server 2022 | Check ECI customer portal; may require a JobBOSS version update first |

**Pre-SQL Server upgrade checklist:**
1. [ ] Note the current JobBOSS version (Help → About).
2. [ ] Log into the ECI customer portal.
3. [ ] Search for the SQL Server compatibility matrix for your JobBOSS version.
4. [ ] Confirm the target SQL Server version is listed as compatible.
5. [ ] If not listed, contact ECI support before proceeding.
6. [ ] Take a full database backup immediately before the SQL Server upgrade.
7. [ ] Schedule the SQL Server upgrade during a planned maintenance window.
8. [ ] Test JobBOSS connectivity immediately after the SQL Server upgrade.

---

## 9.6 JobBOSS Upgrade Process

### 9.6.1 Types of Updates

| Update Type | Description | Frequency | Downtime Required |
|---|---|---|---|
| Minor patch / hotfix | Bug fixes; no schema changes | Monthly or as-needed | Minimal (minutes) |
| Minor version update | New features; possible minor schema changes | Quarterly | 30–60 minutes |
| Major version upgrade | Significant new features; schema changes likely | Annually | 1–4 hours |

### 9.6.2 Pre-Upgrade Checklist

Complete every item on this checklist before beginning any upgrade:

- [ ] Note current version number: **Help → About** → record the version.
- [ ] Notify all users of the maintenance window start and end time.
- [ ] Verify all users are logged out of JobBOSS before beginning.
- [ ] Execute a full SQL Server database backup and verify it completed successfully.
- [ ] Copy all custom Crystal Reports (.rpt files) from the User-Defined directory to a separate backup folder labeled with the current date and version number.
- [ ] Document all ODBC DSN settings (DSN name, server, database name, authentication type).
- [ ] Review the release notes for the target version (available on the ECI customer portal). Read all notes, not just the highlights.
- [ ] Confirm SQL Server version compatibility for the new JobBOSS version.
- [ ] Confirm QuickBooks connector version compatibility if using the QuickBooks integration.
- [ ] Identify any known breaking changes in the release notes that may affect custom reports or integrations.
- [ ] Confirm the upgrade installer has been downloaded and its checksum verified if provided.

### 9.6.3 Upgrade Steps for On-Premise Installation

1. Confirm all users are logged out. In SQL Server, run:

   ```sql
   SELECT DB_NAME(dbid) as DatabaseName, COUNT(dbid) as NumberOfConnections
   FROM sys.sysprocesses
   WHERE dbid > 0
   GROUP BY dbid;
   ```

   Verify the JobBOSS database shows only the expected administrative connections (yours).

2. Execute the upgrade installer provided by ECI. Run as Administrator.
3. Follow the installer prompts. The installer will:
   - Stop the JobBOSS application service (if applicable).
   - Replace application files.
   - Run database schema migration scripts.
   - Restart services.
4. Do not interrupt the installer at any point. If power or connection loss occurs during an upgrade, contact ECI support before attempting to restart the installer.
5. After the installer reports completion, launch JobBOSS.
6. Log in with an administrator account.
7. Navigate through the main menu to confirm the application is functional.

### 9.6.4 Post-Upgrade Verification Checklist

After every upgrade, verify each of the following before returning the system to production use:

**Module Verification:**

- [ ] **Jobs Module:** Open an existing job. Verify routing, materials, and job data display correctly.
- [ ] **Quoting Module:** Open an existing quote. Verify all fields display.
- [ ] **Purchasing Module:** Open an existing PO. Verify vendor, items, and quantities.
- [ ] **Inventory Module:** Check inventory levels on a known part. Verify quantity and bin location.
- [ ] **Accounting Module:** Open AR Aging. Verify customer balances appear correctly.
- [ ] **Scheduling Module:** Open the production schedule. Verify jobs appear in correct sequence.

**Reporting Verification:**

- [ ] Run each custom Crystal Report through **Database → Verify Database** in Crystal Reports designer.
- [ ] Fix any broken field references identified.
- [ ] Run each report against live data and verify output.

**Integration Verification:**

- [ ] If using QuickBooks integration: post a test transaction and verify it syncs to QuickBooks correctly.
- [ ] If using data collection terminals: clock in and out on a terminal and verify the transaction records in JobBOSS.
- [ ] Verify ODBC connections still work (open ODBC Administrator → test each DSN).

**Performance Observation:**

- [ ] Monitor system performance for the first week after upgrade.
- [ ] If queries are noticeably slower after upgrade, run the SQL Server index rebuild maintenance immediately — upgrades sometimes modify indexes in ways that require a manual rebuild pass.

---

## 9.7 System Configuration Best Practices for Die-Mension

### 9.7.1 Go-Live Setup Order

The sequence in which master data is configured matters. Some configuration items depend on others being complete first. Follow this order:

| Step | Task | Dependency | Estimated Time |
|---|---|---|---|
| 1 | Configure Work Centers | None — start here | 2–4 hours |
| 2 | Configure Operations | Work Centers must exist | 2–3 hours |
| 3 | Configure Employees | None | 1–2 hours |
| 4 | Configure Bin Locations | None | 1–2 hours |
| 5 | Configure Units of Measure | None | 30 minutes |
| 6 | Configure Chart of Accounts | QuickBooks COA must be finalized first | 2–4 hours |
| 7 | Configure Customer Classes | None | 30 minutes |
| 8 | Configure Vendor Classes | None | 30 minutes |
| 9 | Import or Enter Part Master Records | UOM must exist; Bin Locations must exist | 4–40 hours depending on part count |
| 10 | Configure Costing Methods on Parts | Part Master must exist | 1–2 hours |
| 11 | Enable Lot Control on Applicable Parts | Part Master must exist | 1 hour |
| 12 | Enter Opening Inventory (Physical Count) | Part Master, Bin Locations must exist | 4–8 hours |
| 13 | Enter Open Purchase Orders | Part Master, Vendors must exist | 2–4 hours |
| 14 | Enter Open Sales Orders and Jobs | Part Master, Customers, Work Centers must exist | 4–16 hours depending on WIP count |
| 15 | Configure QuickBooks Integration | COA must be matched | 2–4 hours |
| 16 | Configure ODBC Connections | SQL Server access required | 1 hour |
| 17 | Configure Crystal Reports (if applicable) | ODBC must work | Variable |
| 18 | Configure Data Collection Terminals | Employees must exist | 2–4 hours |
| 19 | Train Users | All above must be complete | 4–8 hours per user |
| 20 | Go Live | All above verified | — |

### 9.7.2 System Settings to Review Before Go-Live

Navigate to **Administration → System Settings** (or **Table Maintenance → System Preferences**) and review:

| Setting | Recommended Value for Die-Mension | Reason |
|---|---|---|
| Job Number Format | Auto-increment numeric only (e.g., 10001, 10002...) | Eliminates confusion; auto-increment prevents duplication |
| Default Costing Method | Average Cost or Standard Cost | Confirm with Karen/accountant; Average is most common for job shops |
| Default Bin Location | RECEIVING | New parts default to receiving for verification before put-away |
| Sales Tax Settings | Configure per Ohio sales tax requirements | Ohio manufacturing exemptions may apply — consult accountant |
| Default Shipping Method | UPS Ground or Will Call (Die-Mension's typical method) | Speeds order entry |
| Default Payment Terms | Net 30 | Can be overridden per customer |
| Overhead Application Method | Confirm with accountant | Standard, Actual, or None |
| Labor Posting Method | Confirm with accountant | Real-time vs. batch |
| Lot Control Default | Off (enable per part) | Prevents unnecessary lot tracking overhead on non-critical materials |

### 9.7.3 Ongoing System Hygiene Tasks

The following tasks should become part of Karen's regular system administration routine:

**Daily:**
- Review any error or exception notifications from KnowledgeSync (if configured).
- Verify data collection terminal connectivity if shop floor terminals are in use.

**Weekly:**
- Verify the SQL Server automated backup job completed successfully. Check the maintenance plan history in SSMS: **Management → Maintenance Plans → [plan name] → right-click → View History**.
- Review the user account list for any accounts that should be deactivated.

**Monthly:**
- Test a backup restore.
- Run **Database → Verify Database** on any custom Crystal Reports if a JobBOSS update was applied during the month.
- Review Table Maintenance for any stale or incorrect master data (work center rates, employee rates, part master settings).
- Archive closed jobs from active views if the system offers a job archiving function — reduces data volume in active queries.

**Quarterly:**
- Full review of active user accounts — confirm all active users still require access.
- Review work center labor rates and overhead rates — update if rates have changed.
- Review employee labor rates — update for any wage changes.
- Verify all Crystal Reports produce correct output.
- Check database growth — if the database has grown significantly, discuss storage capacity planning with IT or hosting provider.

**Annually:**
- Review the Chart of Accounts with the accountant — confirm all accounts are still needed.
- Review customer and vendor class assignments — are all customers and vendors properly classified?
- Review UOM codes — remove unused codes that create confusion.
- Plan for the next major JobBOSS version upgrade if one is pending.

---

## 9.8 Data Backup Reference for Karen (Non-Technical Summary)

This section provides a plain-language summary of data protection responsibilities specifically for Karen, without requiring technical database knowledge.

### 9.8.1 Cloud (JobBOSS²) Backup Responsibilities

If Die-Mension is on JobBOSS² (cloud-hosted):

| Backup Responsibility | Who Handles It | What Karen Does |
|---|---|---|
| Server hardware backups | ECI | Nothing — ECI owns this |
| Database-level backups | ECI | Nothing — ECI owns this |
| Disaster recovery for server failure | ECI | Nothing — covered by ECI SLA |
| Protection against user error (deleted records) | Karen | Export key data monthly to Excel |
| Business continuity if ECI has an outage | Karen | Maintain Excel exports for continuity |

**Monthly data exports Karen should maintain:**

1. **Customer List:** Export all active customers with contact information to Excel.
2. **Open Jobs List:** Export all open jobs with job number, customer, part, quantity, due date, and status.
3. **Inventory Snapshot:** Export current inventory quantities and values.
4. **Open PO List:** Export all open purchase orders with vendor, expected delivery, and amounts.

Store these exports in a clearly labeled folder on a local machine or cloud storage (OneDrive, Google Drive). Name each file with the export date:

- `Customer_List_2026-02-01.xlsx`
- `Open_Jobs_2026-02-01.xlsx`
- `Inventory_Snapshot_2026-02-01.xlsx`
- `Open_PO_List_2026-02-01.xlsx`

### 9.8.2 On-Premise Backup Responsibilities

If Die-Mension is on the legacy on-premise JobBOSS installation:

> **CRITICAL:** An on-premise installation requires active management of SQL Server backups. If no IT person is assigned to this responsibility, hire an IT consultant to configure automated backups before going live. Operating an on-premise JobBOSS without verified automated backups is an unacceptable business risk. A database corruption or server failure without a current backup would mean loss of all job history, customer records, inventory data, and financial records.

**Karen's on-premise backup checklist:**

- [ ] Confirm with IT or consultant that an automated backup job is configured in SQL Server.
- [ ] Confirm backup files are being written to a location separate from the SQL Server drive (not just a second folder on the same drive — a different physical drive or network location).
- [ ] Confirm backup files are being copied to an off-site or cloud location (a fire that destroys the server also destroys backups on the same machine).
- [ ] Confirm someone runs a test restore at least quarterly.
- [ ] Keep a log of test restore results: date, who performed the test, success or failure, and any issues found.

**Backup log template:**

| Date | Backup Type | Backup File | Restore Tested? | Tested By | Result | Notes |
|---|---|---|---|---|---|---|
| 2026-02-01 | Full | JB_20260201.bak | Yes | Karen | Success | Restore took 12 min |
| 2026-03-01 | Full | JB_20260301.bak | | | | |

### 9.8.3 What to Do If the System Goes Down

If JobBOSS becomes inaccessible for any reason:

**Step 1:** Check if it is a network or connectivity issue (can other applications reach the internet or server?).

**Step 2:** For JobBOSS² (cloud) — check the ECI system status page. ECI publishes service status notifications.

**Step 3:** Contact ECI support:
- Support phone: (refer to ECI customer portal for current support contact numbers)
- Support portal: ECI customer portal → Submit a ticket
- Have your company account number and the current version number ready.

**Step 4:** While waiting for resolution, use the most recent Excel exports (Section 9.8.1) to continue critical operations manually:
- Use the Open Jobs export to continue tracking job status on a whiteboard or in a temporary Excel file.
- Use the Open PO export to answer vendor questions.
- Continue recording any transactions on paper so they can be entered when the system is restored.

**Step 5:** After system restoration, enter all transactions recorded during the outage before closing out the day.

---

*End of Section 8 and Section 9.*



---

No relevant skills apply. Proceeding with the writing task.

---

### 10. TRAINING RESOURCES

#### 10.1 Overview

Becoming proficient in JobBOSS or JobBOSS2 requires deliberate investment in training. ECI Solutions and its partners offer a layered ecosystem of learning options ranging from free self-service video libraries and community academies to paid instructor-led courses and regional workshops. This section catalogs every confirmed training channel, explains how to access each one, and provides guidance on sequencing training for new implementations, individual users, and ongoing professional development.

The training landscape for JobBOSS breaks into four primary tiers:

1. Free self-service resources (videos, webinars, community academy)
2. Paid subscription learning passes (monthly, per-seat pricing)
3. Instructor-led deep dive classes (scheduled, topic-focused)
4. Regional and on-site workshops (hands-on, peer-learning environment)

New implementations should begin with Tier 1 resources to establish vocabulary and orientation, then progress through Tier 2 and Tier 3 as modules go live. Ongoing users benefit most from Tier 3 deep dives targeting specific problem areas identified after go-live.

---

#### 10.2 Free Training Resources

##### 10.2.1 ECI Video Library [CONFIRMED]

ECI Solutions maintains a library of training videos accessible through the resource portal at **resource.ecisolutions.com**. Key characteristics of this library:

- Total video count: 100+ individual videos [CONFIRMED]
- Average video length: 3 to 15 minutes per video [CONFIRMED]
- Format: on-demand, available 24/7
- Content scope: covers core JobBOSS modules including purchasing, inventory, job creation, scheduling, data collection, and reporting

These videos are suitable for new users orienting themselves to a specific module and for experienced users needing a quick reference refresher. Because videos are short and topic-specific, they work well as just-in-time training immediately before performing an unfamiliar task.

Access path: Navigate to resource.ecisolutions.com and locate the JobBOSS section. No separate paid subscription is required for the base video library, though some advanced content may require a Learning Pass. [LIKELY]

##### 10.2.2 ECI Training Portal [CONFIRMED]

The dedicated JobBOSS training portal is located at:

**customerportal.ecisolutions.com/s/training-jobboss**

This portal consolidates training registrations, recorded session archives, and links to deep dive class enrollment. Users with an active ECI customer account can log in and browse available sessions. The portal is the central hub for managing all paid Learning Pass content as well as free recorded sessions.

Training support phone: **(866) 342-8392** [CONFIRMED]

##### 10.2.3 Free Recorded Webinar Sessions [CONFIRMED]

ECI has released at least two sessions as free public resources:

- **Session 1 and Session 2: FREE JobBOSS Training — Tips and Tricks on How to Use JobBOSS Dashboards.** These sessions focus specifically on the dashboard interface, covering navigation, widget configuration, and data interpretation. These are accessible on demand with no Learning Pass required.

- **JobBOSS Material and Scheduling Webinar.** This recorded webinar covers material planning fundamentals and scheduling within JobBOSS. It is available on demand and is particularly useful for shops that are newly implementing material control or struggling with scheduling accuracy. [CONFIRMED]

Both sessions can be accessed through the training portal or the ECI resource site.

##### 10.2.4 TITANS of CNC Academy [CONFIRMED]

The TITANS of CNC Academy is a free external educational platform operated in partnership with ECI Solutions. The academy is accessible at:

**academy.titansofcnc.com**

Background: Titan Gilroy, founder of TITANS of CNC, implemented JobBOSS in his CNC machine shop located in Rocklin, California. Through his direct experience deploying the software in a production environment, he formed a formal alliance with ECI Solutions to create structured educational content for the manufacturing community. [CONFIRMED] The resulting curriculum bridges general shop management principles with JobBOSS-specific software workflows, providing context that purely software-focused training often lacks.

**Shop Management 101 Series** [CONFIRMED]

The 101 series is designed for users who are new to shop management software or to JobBOSS specifically. The series includes the following individual courses:

| Course Title | Notes |
|---|---|
| Introduction to Shop Management | Foundation course; no software prerequisite |
| Order Entry with JobBOSS | Instructor: Andre Weber [CONFIRMED] |
| Quoting with JobBOSS | Covers estimate creation workflow |
| Material Planning with JobBOSS | Covers MRP and purchasing integration |
| Data Collection with JobBOSS | Covers terminal setup and shop floor entry |
| Scheduling with JobBOSS | Covers planning board and scheduling advisor |

**Shop Management 201 Series** [CONFIRMED]

The 201 series advances into professional development topics and specialized modules:

| Course Title | Notes |
|---|---|
| Education and Training | Organizational training strategy for shop personnel |
| Tools and Equipment | Equipment management within a manufacturing context |
| JobBOSS Quality by uniPoint | Integration with uniPoint quality module [CONFIRMED] |

The TITANS of CNC Academy is recommended as a first resource for shop floor supervisors and managers who need conceptual grounding before engaging with software-specific training. Because the content is produced by a practitioner who actually runs a CNC shop, the examples and scenarios are directly relevant to the job shop and make-to-order manufacturing audience.

---

#### 10.3 Paid Training: Learning Pass Options [CONFIRMED]

ECI Solutions offers subscription-based training access through Learning Pass plans. These plans are priced per JobBOSS seat per month and provide access to structured, instructor-led content beyond the free video library.

##### 10.3.1 Elite Learning Pass

**Price:** $29 per month, per JobBOSS seat [CONFIRMED]

**Included content:**

- Live online courses covering the following subject areas: [CONFIRMED]
  - JobBOSS Core (foundational system operation)
  - Accounting (GL integration, financial module)
  - Data Collection (terminal configuration and shop floor entry)
  - Sales Order Processing (customer orders, shipping, invoicing)
  - ShopBOSS (shop floor management module) [CONFIRMED — exact scope UNVERIFIED, see Section 12]
  - Advanced Deep Dives (topic-specific intensive sessions)
  - Crystal Reports (custom report building and modification)
  - Microsoft Office Reporting (Excel and Word integration with JobBOSS data)

- 24/7 access to all previously recorded live sessions [CONFIRMED]
- Monthly live instructor-led sessions scheduled throughout the month [CONFIRMED]

The Elite Learning Pass is the baseline paid option and is appropriate for shops that want structured, current training delivered by ECI-certified instructors. The monthly cadence of live sessions allows users to attend a topic when it aligns with their current implementation phase.

##### 10.3.2 Premium Bundle

**Price:** $79 per month [CONFIRMED]

**Included content:**

- All three Learning Passes [CONFIRMED — note: the research identifies this as "all 3 Learning Passes" but only the Elite Learning Pass is fully documented; the names and contents of the second and third Learning Passes are UNVERIFIED]
- JobBOSS Regional Workshops (see Section 10.5) [CONFIRMED]

The Premium Bundle is the highest-tier subscription option. It is recommended for shops with multiple users across multiple modules who will benefit from the full breadth of content and for implementation teams who want workshop access included in a single subscription.

##### 10.3.3 Multi-Plan Discount Pricing [CONFIRMED]

ECI offers tiered discounts when combining multiple Learning Pass plans:

- Combining 2 plans: **10% discount** applied
- Combining 3 plans: **15% discount** applied

These discounts apply to the monthly per-seat cost and are relevant for shops purchasing training for multiple seat holders who may be enrolled in different plan tiers.

---

#### 10.4 Deep Dive Training Classes [CONFIRMED]

Deep Dive classes are focused, intensive training sessions on specific JobBOSS topics. They are available both through the customer portal and as part of the Elite Learning Pass subscription. Deep Dives are appropriate for users who have completed foundational training and need expert-level instruction on a particular functional area.

Currently available Deep Dive classes include:

##### Material Management Deep Dive [CONFIRMED]

**Objective:** Gain more control over material purchasing and planning by using the full capability of JobBOSS material management.

**Recommended for:** Purchasing managers, inventory controllers, and shop owners who are experiencing stockouts, incorrect costing, or who are not using MRP functionality. This class covers the complete material management workflow from part master setup through bin location management, lot control, reorder points, and the Quick View Material Forecast Utility.

##### Scheduling and Capacity Management Deep Dive [CONFIRMED]

**Objective:** Managing demand versus capacity, achieving accurate scheduling, and measuring shop capacity.

**Recommended for:** Schedulers, shop managers, and anyone responsible for on-time delivery performance. This class addresses the full scheduling workflow including work center setup, routing accuracy, and use of the Planning Board and Whiteboard Scheduler. It also covers the Scheduling Advisor tool for identifying at-risk jobs.

##### Job Control (2-Day Class) [CONFIRMED]

This is a two-day format class, making it the most intensive structured training offering. The extended format is appropriate for shops that are new to JobBOSS and need comprehensive onboarding to the job creation and management workflow, or for shops that have been underutilizing core job control functions.

##### Job Costing Deep Dive [CONFIRMED]

**Objective:** Accurate cost capture and analysis at the job level.

**Recommended for:** Controllers, shop owners, and estimators who need to understand how labor, material, and outside processing costs flow through a job and surface in financial reports.

##### Crystal Reports Advanced Training [CONFIRMED]

**Objective:** Building, modifying, and optimizing custom reports using the Crystal Reports designer connected to the JobBOSS SQL Server database.

**Recommended for:** Any user responsible for custom reporting. See also Section 11 (Common Problems) for known issues with Crystal Reports compatibility after upgrades, and Section 13 for ODBC connection guidance.

##### Microsoft Office Reporting [CONFIRMED]

**Objective:** Connecting JobBOSS data to Microsoft Excel and Word for custom reporting, dashboards, and document automation.

**Recommended for:** Users who prefer Excel-based analysis or need to build reports without a Crystal Reports license.

##### Additional Specialized Topics [CONFIRMED — specific titles UNVERIFIED]

ECI periodically adds new Deep Dive topics based on customer demand. Check the training portal at customerportal.ecisolutions.com/s/training-jobboss for the current catalog.

---

#### 10.5 Regional Workshops [CONFIRMED]

ECI Solutions hosts regional workshops throughout the United States on a monthly schedule. These workshops are distinct from online training in that they provide a hands-on, in-person learning environment.

**Workshop format:** [CONFIRMED]

- **2-Day Introductory Courses:** Full two-day sessions covering JobBOSS fundamentals, suitable for new users or shops implementing JobBOSS for the first time.
- **Advanced Full-Day Classes:** Single-day sessions on specific advanced topics, suitable for existing users seeking to deepen expertise in a particular area.

**What attendees receive:** [CONFIRMED]

- Printed reference manuals provided to all attendees
- Laptop computer provided during class for hands-on exercises (attendees do not need to bring their own)
- Peer-to-peer learning opportunities with other JobBOSS customers from different industries and shop sizes

**Access:** Regional Workshops are included in the Premium Bundle ($79/month). They may also be available for individual registration through the training portal. [LIKELY — individual registration pricing UNVERIFIED]

The peer-to-peer learning component of regional workshops is a frequently cited benefit. Job shop owners and managers often find significant value in sharing implementation experiences and problem-solving approaches with peers who face similar manufacturing challenges.

---

#### 10.6 Reference Book: Mastering JobBoss [CONFIRMED]

A comprehensive reference book is available for practitioners who prefer structured, self-paced reading:

- **Title:** Mastering JobBoss: A Comprehensive Guide for Job Shops and Make-to-Order Manufacturers [CONFIRMED]
- **Author:** Innoware PJP [CONFIRMED]
- **Publication Year:** 2024 [CONFIRMED]
- **ISBN:** 9798322488682 [CONFIRMED]
- **Availability:** Amazon [CONFIRMED]

The book is published independently and covers JobBOSS from a practitioner perspective. The full table of contents has not been independently verified for this manual. [UNVERIFIED] Readers should confirm the book's coverage of their specific version of JobBOSS before purchasing, as the software is actively developed and content may not reflect the most current release.

---

#### 10.7 Training Sequence Recommendations [LIKELY]

The following sequence is recommended based on implementation best practices. Specific sequencing is advisory and should be adjusted based on your shop's implementation phase and prior ERP experience.

**Phase 1 — Pre-Go-Live (Weeks 1-4)**
1. All users: TITANS of CNC Academy Shop Management 101 series (free, self-paced)
2. System administrator and implementation lead: ECI video library — Core and Setup modules
3. Implementation lead: Job Control 2-Day Class or regional introductory workshop

**Phase 2 — Go-Live Support (Weeks 4-8)**
1. Subscribe to Elite Learning Pass for key users
2. Attend live monthly sessions aligned with active modules
3. Purchasing/inventory staff: Material Management Deep Dive
4. Schedulers: Scheduling and Capacity Management Deep Dive

**Phase 3 — Optimization (Month 3 and Beyond)**
1. Controller or owner: Job Costing Deep Dive
2. Report developer: Crystal Reports Advanced Training
3. All staff: review free Dashboard Tips and Tricks sessions
4. Premium Bundle: consider for shops with ongoing training needs across multiple roles

---

### 11. COMMON PROBLEMS AND SOLUTIONS

#### 11.1 Overview

This section documents the most frequently encountered problems in JobBOSS implementations, organized by functional category. Each entry describes the symptom, the most common root cause, and the recommended corrective action. Where the root cause or solution involves a known software behavior that may be counterintuitive, additional context is provided.

A recurring theme across all categories is that problems attributed to the software are most often the result of configuration decisions made early in the implementation — particularly costing method selection, lot control enablement, and the presence or absence of a dedicated system administrator. These foundational decisions are difficult or impossible to reverse after transactions have been entered.

---

#### 11.2 Material Control Problems

##### Problem: Material Quantities Not Tracking Correctly [CONFIRMED]

**Symptom:** Inventory quantities in JobBOSS do not match physical counts. On-hand quantities appear incorrect after receiving or issuing material.

**Root Cause 1: Bin locations not configured before first receipt.** [CONFIRMED]
JobBOSS tracks material quantities at the bin location level. If bin locations are not created and assigned to parts before the first receiving transaction, the system has no valid location to post the quantity to, resulting in tracking discrepancies that are difficult to reconcile retroactively.

**Corrective Action:** Before entering any receiving transactions, navigate to the part master and verify that at least one bin location is assigned to each stocked part. Create bin locations in Table Maintenance if they do not exist. If transactions have already been entered without bin locations, a physical count adjustment will be required to establish a clean baseline.

**Root Cause 2: Lot control not enabled before first receipt.** [CONFIRMED]
If lot control is required (for heat number tracking, traceability, or quality purposes), it must be enabled on the part master before the first receiving transaction. Enabling lot control after transactions exist does not retroactively assign lot numbers to existing inventory, creating a split record that cannot be fully reconciled within the system.

**Corrective Action:** Decide which parts require lot tracking before go-live. Enable lot control on those part records and verify the setting before receiving any quantity. Once material has been received without lot control and lot control is later enabled, expect a manual reconciliation effort to bring historical records in line.

---

##### Problem: Wrong Costing Method Applied [CONFIRMED]

**Symptom:** Job cost reports show unexpected material costs. Inventory valuations appear inconsistent. Financial reports do not match expectations.

**Root Cause:** The costing method (FIFO, LIFO, Standard Cost, or Average Cost) was set incorrectly, or was changed after receiving transactions had already been entered. [CONFIRMED]

**Explanation of costing methods:**
- **FIFO (First In, First Out):** Material cost is assigned to jobs in the order it was received. Oldest receipts are consumed first.
- **LIFO (Last In, First Out):** Most recent receipt cost is consumed first. Less common in manufacturing; may have accounting implications.
- **Standard Cost:** A predetermined cost per unit is used regardless of actual purchase price. Variances are tracked separately.
- **Average Cost:** A running weighted average of all received costs is maintained and applied to issues.

**Critical Warning:** [CONFIRMED] The costing method is extremely difficult to change after transactions exist in the system. Changing the costing method after material has been received and issued to jobs will produce incorrect historical cost data and may require a complete inventory re-valuation. This decision must be made during initial configuration, before the first receiving transaction is entered, and should involve input from the company's accountant or controller.

**Corrective Action:** If the wrong costing method is in place and transactions already exist, consult with your ECI implementation consultant before making any changes. In some cases, a period-end inventory adjustment combined with a method change may be viable; in other cases, the cleanest solution is to accept the current method going forward and adjust accounting entries to compensate.

---

##### Problem: Not Using MRP / Stockouts Occurring [CONFIRMED]

**Symptom:** Shop runs out of material unexpectedly. Purchase orders are created reactively after jobs are already started rather than proactively. The system is not generating purchase alerts.

**Root Cause 1: The Quick View Material Forecast Utility is not being used.** [CONFIRMED]
JobBOSS includes a built-in MRP screen called the Quick View Material Forecast Utility. Many shops are unaware of this tool or have not incorporated it into their purchasing workflow, resulting in manual, reactive purchasing decisions.

**Corrective Action:** Train the purchasing function on the Quick View Material Forecast Utility. Access it through the Inventory module. The screen displays all parts that are at or below their reorder point or that have projected demand from open jobs. It also supports automatic PO generation directly from the screen. Incorporate a daily or weekly review of this screen into the purchasing routine.

**Root Cause 2: Reorder points not set on part records.** [CONFIRMED]
The system cannot generate alerts or trigger MRP calculations for parts that do not have a reorder point defined. If part records were created without reorder points, the Quick View Material Forecast Utility will not flag those parts even if they are at zero quantity.

**Corrective Action:** Review all stocked part records and establish reorder points based on lead time and average consumption. Even a conservative reorder point is better than none. Consider also setting economic order quantities (EOQ) if purchasing patterns support it. [LIKELY — EOQ field existence UNVERIFIED]

---

##### Problem: Purchase to Job vs. Purchase to Stock Confusion [CONFIRMED]

**Symptom:** Material costs appear on wrong jobs. Inventory levels are incorrect because material purchased for specific jobs is being posted to general stock, or vice versa.

**Root Cause:** JobBOSS supports two distinct purchasing workflows — purchasing material directly to a job (job-specific receipt) and purchasing material to general stock inventory (stock receipt). When operators or purchasing staff use the wrong workflow, material costs do not flow through the system as intended.

**Corrective Action:** Establish a shop policy defining when each method is used. Typically: standard stocked materials are purchased to stock; custom or one-time materials are purchased to job. Train all purchasing and receiving personnel on this distinction and document it in your internal procedures. Configure the default purchasing method in system settings if the shop consistently uses one method. [LIKELY — configurable default UNVERIFIED]

---

#### 11.3 QuickBooks Integration Problems

##### Problem: Sync Failures — Duplicate Vendor/Customer IDs [CONFIRMED]

**Symptom:** The QuickBooks synchronization process fails with an error related to duplicate or conflicting IDs. Vendors or customers fail to transfer correctly.

**Root Cause:** JobBOSS allows the same company to have both a vendor record and a customer record using the same ID (for example, a company that both supplies material and purchases finished goods). QuickBooks Online does not permit this — each entity must have a unique identifier. [CONFIRMED]

**Corrective Action:** Before configuring the QuickBooks integration, audit all vendor and customer records in JobBOSS. Identify any cases where the same ID is used for both a vendor and a customer. Assign unique IDs to one or both records before activating the sync. The convention of appending a suffix (e.g., "-V" for vendor, "-C" for customer) is a common approach, though any unique naming scheme that your accounting team approves is acceptable. [LIKELY — suffix convention]

---

##### Problem: Multiple Addresses Not Syncing Correctly [CONFIRMED]

**Symptom:** Customer contact information or shipping addresses are lost or truncated after syncing to QuickBooks Online.

**Root Cause:** JobBOSS supports multiple addresses and contact records per customer. QuickBooks Online only allows a single customer contact and address per record. The integration cannot map the additional JobBOSS address records to a valid QuickBooks Online field. [CONFIRMED]

**Corrective Action:** Before sync, designate the primary address and contact for each customer in JobBOSS as the record that will map to QuickBooks Online. Accept that secondary addresses will not sync and maintain them only within JobBOSS. Document this limitation in your accounting procedures so that billing staff know to verify addresses in JobBOSS rather than relying solely on QuickBooks.

---

##### Problem: Revenue Posting Not Reconciling [CONFIRMED]

**Symptom:** Financial reports between JobBOSS and QuickBooks do not reconcile. Revenue figures differ between systems.

**Root Cause:** Revenue posting within the integration must be set to "product code" for financial reports to reconcile correctly. If a different posting method is selected, the integration will post revenue to incorrect accounts, requiring periodic manual journal entries to correct. [CONFIRMED]

**Corrective Action:** Verify the revenue posting setting in the QuickBooks integration configuration. Set to "product code." If incorrect posting has already occurred, periodic journal entries will be required to true up the accounts. Involve your accountant in reviewing the current posting configuration before making changes, as altering this mid-period will affect in-period financial reports.

---

##### Problem: QuickBooks Desktop vs. QuickBooks Online Behavior Differences [CONFIRMED]

**Symptom:** Integration behaves differently than documented or than experienced in a previous installation. Features available in one version do not appear in another.

**Root Cause:** JobBOSS integrates with both QuickBooks Desktop and QuickBooks Online, but the two integrations are not identical. Certain fields, behaviors, and limitations differ between the two platforms. [CONFIRMED]

**Corrective Action:** Before configuring the integration, confirm which version of QuickBooks your company uses. Do not assume that documented procedures for QuickBooks Desktop apply to QuickBooks Online or vice versa. When seeking support or training for the integration, specify your QuickBooks version explicitly.

---

#### 11.4 Performance and Database Problems

##### Problem: Slow Response Times / System Crashes [CONFIRMED]

**Symptom:** JobBOSS responds slowly to user actions, particularly when opening large records, running reports, or when multiple users are active simultaneously. In severe cases, the application crashes.

**Root Cause 1: Database maintenance not performed.** [CONFIRMED]
JobBOSS runs on Microsoft SQL Server. SQL Server requires periodic maintenance tasks — specifically index rebuilds and statistics updates — to maintain query performance. Shops that do not have a SQL Server maintenance plan in place will experience progressively degrading performance over time as indexes fragment and statistics become stale.

**Corrective Action:** Establish a SQL Server maintenance plan that performs index rebuilds and statistics updates on a scheduled basis (weekly is a common interval for actively used databases). This can be configured through SQL Server Management Studio using the SQL Server Maintenance Plan Wizard. If your organization does not have SQL Server expertise internally, engage an IT consultant or ECI support. [LIKELY — weekly interval recommendation]

**Root Cause 2: Crystal Reports queries not optimized.** [CONFIRMED]
Crystal Reports connected to the JobBOSS database via ODBC can generate resource-intensive queries, particularly for reports that scan large transaction tables without appropriate date filters or indexes.

**Corrective Action:** Review custom Crystal Reports for missing date range parameters. Adding a date filter to reports that scan the transaction register or similar large tables can dramatically reduce query execution time. Consult the Crystal Reports Advanced Training deep dive if custom report optimization is needed.

**Root Cause 3: Simultaneous user load.** [CONFIRMED]
Heavy concurrent usage, particularly during morning startup periods when all users log in within a short window, can cause temporary performance degradation.

**Corrective Action:** Verify that the SQL Server is hosted on dedicated hardware with adequate RAM and CPU resources. JobBOSS is not designed to run on underpowered hardware in a multi-user environment. Consult ECI's system requirements documentation for current hardware recommendations. [LIKELY — dedicated hardware recommendation]

---

##### Problem: SQL Server Version Compatibility [CONFIRMED]

**Symptom:** Uncertainty about whether upgrading SQL Server will break JobBOSS functionality.

**Root Cause:** Not all versions of JobBOSS have been qualified against all versions of SQL Server. In particular, SQL Server 2017 compatibility requires verification before upgrading. [CONFIRMED]

**Corrective Action:** Do not upgrade SQL Server without first confirming that your installed version of JobBOSS supports the target SQL Server version. Contact ECI support or check the customer portal for the current compatibility matrix before proceeding with any SQL Server upgrade. [CONFIRMED]

---

#### 11.5 Report Generation Problems

##### Problem: Crystal Reports Break After JobBOSS Upgrade [CONFIRMED]

**Symptom:** Custom Crystal Reports that worked correctly before a JobBOSS upgrade now display errors, return no data, or show incorrect data after the upgrade is applied.

**Root Cause:** JobBOSS upgrades, particularly major version upgrades, may include changes to database table names, field names, or field data types in the underlying SQL Server database. Custom Crystal Reports that reference specific table and field names by their exact names will break if those names change. [CONFIRMED]

**Corrective Action:** After any JobBOSS upgrade, test all custom Crystal Reports before restoring full production operation. If a report fails, open it in the Crystal Reports designer, navigate to Database > Verify Database, and allow Crystal Reports to identify changed or missing fields. Update field references as needed. Maintain a test environment where upgrades can be applied and reports verified before rolling out to production. [LIKELY — Database > Verify Database menu path]

---

##### Problem: Limited Built-In Report Customization [CONFIRMED]

**Symptom:** Standard JobBOSS reports do not display the fields, layout, or formatting required for shop floor, customer-facing, or management reporting needs.

**Root Cause:** The built-in report templates in JobBOSS have limited customization options within the application interface. Meaningful layout and content changes require the Crystal Reports designer. [CONFIRMED]

**Corrective Action:** Obtain a Crystal Reports license and connect to the JobBOSS SQL Server database via ODBC. All JobBOSS reports are Crystal Reports files that can be opened, modified, and saved back to the system. The Crystal Reports Advanced Training deep dive is recommended for users who need to perform significant report customization.

---

#### 11.6 Data Entry and Shop Floor Problems

##### Problem: Terminal Data Collection Fails on Alpha Job Numbers [CONFIRMED]

**Symptom:** Shop floor data collection terminals cannot locate or accept certain job numbers. Operators receive errors when attempting to enter time against specific jobs.

**Root Cause:** JobBOSS data collection terminals require numeric-only job numbers. Job numbers that include letters or special characters are not compatible with terminal data entry. [CONFIRMED]

**Corrective Action:** If your shop uses data collection terminals, establish a job numbering convention that uses numeric characters only. If alpha job numbers already exist in the system, those jobs cannot be entered at the terminal — operators must use the JobBOSS application interface for time entry against those jobs, or the jobs must be renumbered if the system allows it. [LIKELY — renumbering feasibility] Going forward, configure JobBOSS to auto-generate numeric job numbers to prevent recurrence.

---

##### Problem: Setup Time and Run Time Entered Incorrectly at Terminal [CONFIRMED]

**Symptom:** Job cost reports show all time as run time. Setup time is not being captured separately. This leads to inaccurate labor cost analysis and scheduling data because setup and run are billed and costed at different rates in many shops.

**Root Cause:** Operators at the terminal are not distinguishing between setup and run time during data entry. Either training is insufficient or the terminal workflow is confusing enough that operators default to entering everything as run. [CONFIRMED]

**Corrective Action:** Provide specific training to shop floor operators on the difference between setup time entry and run time entry at the terminal. Post visual reference cards at each terminal. Consider whether your JobBOSS data collection configuration presents the setup/run selection clearly enough; if the interface is unclear, consult ECI support about terminal configuration options. [LIKELY — visual reference cards suggestion]

---

##### Problem: BATCH Function Misuse Defeating Real-Time Visibility [CONFIRMED]

**Symptom:** Management cannot see real-time job status or labor activity during the shift. All time entries appear at the end of the day or shift rather than throughout the workday.

**Root Cause:** The BATCH function in JobBOSS data collection allows operators to enter time retroactively for multiple operations in a single session. When operators use BATCH entry for all time reporting rather than entering time at the point of work, the real-time visibility benefit of the data collection system is negated. [CONFIRMED]

**Corrective Action:** Reserve BATCH entry for situations where real-time entry was not possible (equipment downtime, new employee onboarding, etc.). Establish a shop floor policy requiring operators to clock in and out of jobs in real time. Implement accountability processes — supervisors reviewing daily labor entries — to identify and correct retroactive entry patterns.

---

#### 11.7 Module Configuration and Implementation Mistakes

##### Problem: Scheduling Data Is Inaccurate [CONFIRMED]

**Symptom:** The JobBOSS scheduling system produces schedules that do not reflect actual shop conditions. Jobs show as on-schedule when they are late, or lead times generated by the system do not match reality.

**Root Cause:** Scheduling accuracy is directly dependent on the accuracy of the supporting data — specifically work center capacities, routing times (setup and run), and current job status. If any of these data sources are inaccurate or incomplete, the scheduling output will be correspondingly inaccurate. The principle is "garbage in, garbage out." [CONFIRMED]

**Corrective Action:** Before relying on the scheduling module, audit the following data elements:
1. Work center definitions: Are all machines and work areas defined? Are their capacity hours correct?
2. Routing times: Are standard setup and run times entered on routings? Are they based on actual observed times or engineering estimates that have not been validated?
3. Job status: Is the data collection system feeding accurate real-time operation completion data, or is job status being updated manually and infrequently?

Address each data quality issue before trusting the schedule output.

---

##### Problem: No Dedicated System Administrator [CONFIRMED]

**Symptom:** JobBOSS implementation stalls, data quality degrades over time, users develop workarounds outside the system, and modules that were configured at go-live fall into disuse.

**Root Cause:** Not having a dedicated person responsible for managing JobBOSS from day one is cited as the number one implementation failure factor. [CONFIRMED] When no one owns the system, configuration decisions are not enforced, new employees are not trained, and the software investment is not fully realized.

**Corrective Action:** Designate a specific individual as the JobBOSS system owner before go-live. This person does not need to be a full-time IT role — in smaller shops it is often the owner, controller, or office manager — but they must have defined responsibility for system configuration, user training, data quality, and software updates. Document this role in the shop's organizational structure.

---

##### Problem: Skipping Implementation Phases [CONFIRMED]

**Symptom:** After going live, the system shows zero inventory balances despite the shop having physical inventory. Open purchase orders visible in the prior system do not appear in JobBOSS. Historical job costs are not available.

**Root Cause:** The implementation process includes required data migration steps — specifically entering open purchase orders and establishing inventory balances — that are sometimes skipped in an effort to accelerate the go-live date. Skipping these steps means the system starts with an incomplete picture of shop status. [CONFIRMED]

**Corrective Action:** Do not abbreviate the data migration phase of implementation. At minimum, enter all open purchase orders and perform an inventory count that establishes starting balances in JobBOSS before going live. Open sales orders and open jobs from the prior system should also be entered if the shop needs historical tracking continuity. This effort takes time but is essential for the system to be immediately useful after go-live.

---

#### 11.8 User Adoption Problems

##### Problem: Low Shop Floor Compliance with Data Entry [CONFIRMED]

**Symptom:** Data collection terminals are installed but operators are not entering time consistently. Management cannot see accurate job status. Labor cost reports are incomplete.

**Root Cause:** Shop floor operators may find the data entry process unfamiliar or inconvenient, particularly if they are accustomed to paper-based time tracking. Without accountability processes, compliance tends to degrade after the initial go-live period. [CONFIRMED]

**Corrective Action:** Implement a supervisor review process in which daily labor entries are reviewed against expected activity. Establish clear expectations with operators that data entry is a required part of their job, not optional. Connect data entry compliance to performance reviews if necessary. Celebrate early wins where the data collection system demonstrably helped the shop — for example, using job status visibility to prevent a late shipment.

---

##### Problem: Support Response Delays [CONFIRMED]

**Symptom:** Support tickets submitted to ECI take one to two days to receive a response. Production issues that need immediate resolution cannot wait for ticket-based support.

**Root Cause:** ECI's standard support channel operates on a ticket system with an average response time of one to two days. [CONFIRMED] This is appropriate for non-urgent configuration questions but is insufficient for critical production-blocking issues.

**Corrective Action:** For non-urgent issues, the ticket system is appropriate. For time-sensitive issues, use the following resources while waiting for ticket response:
1. The ECI customer community forum — other customers may have encountered and solved the same issue
2. The training portal — recorded sessions may address the issue
3. The ECI resource site — quick reference materials may provide an immediate answer
4. Phone support at (866) 342-8392 for critical issues [CONFIRMED]

---

### 12. GLOSSARY

The following terms are used throughout this manual and within the JobBOSS application. Terms are defined in the context of JobBOSS and make-to-order manufacturing. Where a term has a general industry meaning that differs from the JobBOSS-specific usage, both meanings are noted.

---

**Average Cost** [CONFIRMED]
An inventory costing method in which material is valued at a running weighted average of all received costs. When material is issued to a job, the current average cost is applied. The average cost updates with each new receipt, blending the new purchase price into the existing average. Used when purchase prices fluctuate and the shop wants to smooth cost variations rather than track individual receipt costs. Must be selected before the first receiving transaction. See also: FIFO, LIFO, Standard Cost.

---

**Bin Location** [CONFIRMED]
A defined physical storage location within the facility, tracked in the JobBOSS inventory system. Bin locations are created in Table Maintenance and assigned to part records. Material quantities are tracked at the bin location level, allowing the system to show not only how much of a part is on hand but also where it is physically stored. Bin locations must be configured before the first receiving transaction for accurate quantity tracking. Example bin location designations: "SHELF-A1," "RACK-3B," "FLOOR-MEZZANINE."

---

**BOM (Bill of Materials)** [CONFIRMED]
A structured list of all components, raw materials, and subassemblies required to manufacture a part or product, along with the required quantity of each. In JobBOSS, the BOM is associated with the part master and drives material requirements calculations in MRP. BOMs can be entered manually or imported from CAD software using the CAD BOM Import feature. A BOM defines what materials are needed; the Routing defines the operations required to transform those materials into the finished part.

---

**CAD BOM Import** [CONFIRMED]
A JobBOSS feature that allows the import of a Bill of Materials directly from CAD software into the JobBOSS part master. This eliminates manual re-entry of component lists from engineering drawings and reduces the risk of transcription errors. Specific CAD formats and import procedures are version-dependent. [LIKELY — exact supported formats UNVERIFIED]

---

**CAPA (Corrective and Preventive Action)** [CONFIRMED]
A formal quality management process for documenting the root cause of a defect or nonconformance (corrective action) and implementing changes to prevent recurrence (preventive action). In JobBOSS, CAPA documentation is typically handled through the uniPoint quality module integration. [LIKELY — specific uniPoint feature scope UNVERIFIED] CAPA records create an auditable trail from defect identification through resolution, which is required for ISO 9001 and AS9100 compliance.

---

**Collection Terminal** [CONFIRMED]
The JobBOSS-specific term for a data collection device as configured within the application. Collection Terminals are defined and configured in Table Maintenance, where defaults such as the default work center, operator prompts, and job number format are established. The Collection Terminal record in JobBOSS corresponds to a physical Data Collection Terminal device on the shop floor. See also: Data Collection Terminal.

---

**Crystal Reports** [CONFIRMED]
A report design and generation tool developed by SAP and used by JobBOSS as its primary custom reporting platform. Crystal Reports connects to the JobBOSS SQL Server database via ODBC to retrieve and format data. All standard JobBOSS reports are Crystal Reports files (.rpt) and can be opened in the Crystal Reports designer for modification. Custom reports built in Crystal Reports can be saved to the User Defined section of the JobBOSS reports menu. Crystal Reports requires a separate license from SAP. After JobBOSS upgrades, custom Crystal Reports should be verified and may require updates to field references if table or field names have changed.

---

**Data Collection Terminal** [CONFIRMED]
A hardware device installed on the shop floor that allows operators to enter job time, operation completions, and material transactions without accessing a full JobBOSS workstation. Terminals display prompts for job number, operation, employee ID, and time entry. Data Collection Terminals require numeric-only job numbers for compatibility. See also: Collection Terminal (the JobBOSS configuration record for the device).

---

**FIFO (First In, First Out)** [CONFIRMED]
An inventory costing method in which the cost of the oldest received material is assigned first when material is issued to a job or sold. Under FIFO, if 100 units were received at $5.00 and then 100 more at $6.00, the first 100 units issued will be costed at $5.00 and the next at $6.00. FIFO most closely approximates the physical flow of material in most manufacturing environments. Must be selected before the first receiving transaction. See also: LIFO, Average Cost, Standard Cost.

---

**Heat Number** [CONFIRMED]
A lot identification number assigned to a specific batch of steel or metal alloy by the producing mill, recorded on a mill certification document that accompanies the material. The heat number uniquely identifies the melt from which the material was produced and provides traceability to chemical composition and mechanical property test results. In JobBOSS, heat numbers are tracked using the lot control system — the lot number assigned at receiving is the heat number. The receiving record becomes the audit trail linking the heat number to the jobs and parts produced from that material. Required for traceability in aerospace, defense, and other regulated industries.

---

**Job / Work Order** [CONFIRMED]
The central document in JobBOSS that tracks the production of a specific quantity of a part or product. The job record contains the part number, quantity, required date, routing, material requirements, estimated costs, and actual costs as they accumulate. All labor, material, and outside processing transactions are posted against the job number. Jobs are the primary unit of cost collection in JobBOSS and are the basis for job costing analysis and invoicing. Also called a Work Order in general manufacturing terminology; the terms are used interchangeably in most contexts.

---

**KnowledgeSync** [CONFIRMED]
An add-on module for JobBOSS that provides automated alerting and notification capabilities. KnowledgeSync monitors the JobBOSS database for defined conditions — such as a job falling behind schedule, a purchase order becoming overdue, or an inventory item dropping below reorder point — and automatically sends email or other notifications to designated recipients. KnowledgeSync enables proactive management responses without requiring users to manually monitor the system. It is a separately licensed add-on to the base JobBOSS application.

---

**LIFO (Last In, First Out)** [CONFIRMED]
An inventory costing method in which the cost of the most recently received material is assigned first when material is issued. Under LIFO, if 100 units were received at $5.00 and then 100 more at $6.00, the first 100 units issued will be costed at $6.00. LIFO is less common in manufacturing environments and may have specific accounting and tax implications. In the United States, LIFO is permitted under GAAP but not under IFRS. Must be selected before the first receiving transaction. See also: FIFO, Average Cost, Standard Cost.

---

**Lot Number** [CONFIRMED]
An identifier assigned to a specific batch or quantity of material or finished parts for the purpose of traceability. In JobBOSS, lot numbers are assigned at the time of receiving when lot control is enabled on the part record. Lot numbers allow the system to track which specific batch of material was used on which jobs, and which finished parts were produced from that batch. Lot control must be enabled before the first receiving transaction. See also: Heat Number, Serial Number.

---

**Material Transaction User Tracking Utility** [CONFIRMED]
A JobBOSS utility that records and displays material transactions at the individual user level. This tool allows management to audit who performed specific receiving, issuing, or adjustment transactions and when those transactions occurred. Useful for investigating discrepancies in inventory records and for implementing accountability in the material management function.

---

**MRP (Material Requirements Planning)** [CONFIRMED]
A systematic method for calculating what materials are needed, in what quantities, and by what date, based on open job requirements, current inventory levels, and scheduled receipts. In JobBOSS, MRP functionality is accessed through the Quick View Material Forecast Utility. MRP compares material demanded by open jobs against available inventory and outstanding purchase orders, identifying shortages and allowing purchase orders to be generated to cover projected deficits. Effective MRP requires accurate part master records, current inventory quantities, and defined reorder points.

---

**NC (Non-Conformance)** [CONFIRMED]
A quality event in which a part, material, or process does not meet the specified requirement. Non-conformance records document the nature of the defect, the affected quantity, the disposition decision (use-as-is, rework, scrap, return to vendor), and the associated job or purchase order. In JobBOSS, non-conformance tracking is typically handled through the uniPoint quality module integration. [LIKELY — base JobBOSS NC capability without uniPoint UNVERIFIED]

---

**ODBC (Open Database Connectivity)** [CONFIRMED]
A standard application programming interface for connecting software applications to database management systems, regardless of the specific database platform. JobBOSS stores all data in a Microsoft SQL Server database. ODBC connections to the JobBOSS SQL Server database enable external tools — including Crystal Reports, Microsoft Excel, Microsoft Access, and custom applications — to read JobBOSS data directly. Establishing an ODBC connection requires the SQL Server name or IP address, the database name, and appropriate SQL Server credentials. See Section 13 for connection guidance.

---

**Outside Processing** [CONFIRMED]
A routing operation in which work on a job is performed by an external subcontractor rather than within the shop. Common examples include heat treating, plating, painting, and specialized machining operations. In JobBOSS, outside processing operations on a routing automatically generate a purchase order to the designated subcontractor when the job is released. The outside processing PO tracks the material or parts sent out and the cost of the subcontracted service. Cost is posted back to the job upon receipt.

---

**Planning Board** [CONFIRMED]
A scheduling interface within JobBOSS that presents job operations in a drag-and-drop visual format, allowing schedulers to manually adjust operation sequences and assignments. The Planning Board displays work center loads and job timelines, providing a visual representation of the schedule that can be modified interactively. Changes made on the Planning Board update the underlying job records. See also: Whiteboard Scheduler, Scheduling Advisor.

---

**Quick View Material Forecast Utility** [CONFIRMED]
The primary MRP screen in JobBOSS, accessible through the Inventory module. This utility compares material demand from open jobs against available inventory and outstanding purchase orders, displaying a list of parts that are short or at risk of shortage. From this screen, the user can review each shortage and automatically generate purchase orders to cover the deficit. Effective use of this utility requires accurate on-hand quantities, current job requirements, and defined reorder points on part records. Incorporating regular review of this screen into the purchasing workflow is the primary method for preventing material stockouts.

---

**Reorder Point** [CONFIRMED]
A minimum on-hand quantity threshold defined on a part record. When the on-hand quantity for a part falls to or below the reorder point, the system flags the part in the Quick View Material Forecast Utility and may trigger an automatic purchase order depending on configuration. Reorder points must be set on each stocked part record for the MRP system to generate alerts. Setting reorder points requires knowledge of the part's lead time and average consumption rate.

---

**RFQ (Request for Quotation)** [CONFIRMED]
A purchasing document sent to one or more vendors requesting pricing and lead time information for a specified material or service. In JobBOSS, RFQs can be generated from material requirements and sent to vendors for price comparison before issuing a purchase order. RFQ responses can be captured in the system and used to populate purchase order pricing.

---

**Routing** [CONFIRMED]
An ordered sequence of manufacturing operations required to produce a part, defining what work is performed, at which work center, in what sequence, and for how long (setup time and run time per piece). The routing is the central driver of JobBOSS scheduling calculations — it tells the system how long each step takes and where it must be performed. Routing accuracy directly determines scheduling accuracy. Inaccurate routing times produce unreliable schedules regardless of how well the scheduling module is configured.

---

**Scheduling Advisor** [CONFIRMED]
A JobBOSS tool that analyzes the current schedule and identifies jobs that are at risk of falling behind schedule based on remaining work, available capacity, and required due dates. The Scheduling Advisor provides a prioritized list of jobs requiring scheduling attention, allowing schedulers to focus intervention on the highest-risk jobs. See also: Planning Board, Whiteboard Scheduler.

---

**Serial Number** [CONFIRMED]
A unique identifier assigned to a specific individual part or unit produced. Unlike a lot number (which identifies a batch), a serial number identifies one specific piece. Serial number tracking provides full traceability of an individual part through production — which job it was made on, which materials were used, which operators processed it, and when each step occurred. Required for traceability in regulated industries (aerospace, defense, medical). Serial number control must be enabled on the part record before production begins.

---

**ShopBOSS** [CONFIRMED — exact functional scope UNVERIFIED]
A module or feature set within JobBOSS related to shop floor management. ShopBOSS is referenced in the Elite Learning Pass curriculum as a distinct training topic. The exact functional boundaries of ShopBOSS relative to the base JobBOSS application — specifically whether it refers to a separately licensed module, a specific interface, or a branded name for the shop floor execution capabilities — have not been independently verified for this manual. [UNVERIFIED] Users should consult ECI documentation or their ECI account representative for current ShopBOSS scope and licensing.

---

**Stale Inventory Report** [CONFIRMED]
A JobBOSS report that displays inventory items that have had no transactions (no receipts, no issues, no adjustments) since a user-specified date. This report is used to identify slow-moving or obsolete inventory for review and potential write-off. Periodic review of the Stale Inventory Report is a recommended practice for inventory management and year-end physical count preparation.

---

**Standard Cost** [CONFIRMED]
An inventory costing method in which a predetermined, fixed cost per unit is established for each part and used for all material transactions regardless of actual purchase price. Variances between the standard cost and actual purchase price are tracked separately as purchase price variances (PPV). Standard costing simplifies budgeting and cost analysis but requires periodic review and update of standard costs to remain meaningful. Must be selected before the first receiving transaction. See also: FIFO, LIFO, Average Cost.

---

**Transaction Register Report** [CONFIRMED]
A comprehensive log of all material transactions recorded in JobBOSS over a specified time period, including receipts, issues, adjustments, and transfers. The Transaction Register Report is used for auditing inventory activity, investigating discrepancies, and reconciling inventory values for accounting period-end close. It can be filtered by date range, part number, transaction type, and other criteria.

---

**Traveler** [CONFIRMED]
A physical printed document that accompanies a job through the shop floor, providing operators at each work center with the information they need to perform their assigned operation. A typical Traveler includes the job number, part number and description, quantity, routing with operation sequence, setup and run instructions, material requirements, and inspection criteria. The Traveler is printed from JobBOSS and serves as the primary communication tool between the office and the shop floor in shops that do not use fully paperless systems. In shops using data collection terminals or electronic job status, the Traveler may be supplemented or replaced by digital work instructions.

---

**Whiteboard Scheduler** [CONFIRMED]
An advanced scheduling interface in JobBOSS that provides real-time visual scheduling with interactive adjustment capabilities. The Whiteboard Scheduler is distinct from the Planning Board in that it is designed for real-time scheduling decisions with a more immediate visual feedback loop. [LIKELY — specific feature differentiation from Planning Board UNVERIFIED] See also: Planning Board, Scheduling Advisor.

---

**Work Center** [CONFIRMED]
A defined machine, production area, process capability, or resource within the shop that is tracked in JobBOSS for scheduling and capacity purposes. Work Centers are created in Table Maintenance and referenced on routings. Each Work Center has defined capacity (available hours per shift or day) that the scheduling system uses to calculate load and available capacity. Work Centers can represent individual machines, groups of similar machines, manual assembly areas, inspection stations, or any other resource whose capacity constrains production.

---

### 13. QUICK REFERENCE FOR AI ASSISTANTS

#### 13.1 Purpose and Usage

This section is designed to provide structured, accurate question-and-answer mappings for use by AI assistants, chatbots, help desk tools, and other automated support systems that respond to JobBOSS user queries. Each entry maps a common user question to a concise, actionable answer. Confidence levels are indicated for each entry.

When an AI assistant is processing a user query about JobBOSS, the appropriate response pattern is: identify the category of the question, locate the relevant mapping in this section, and supplement with detailed information from the relevant numbered section of this manual.

---

#### 13.2 Material Management

**Q: How do I receive material in JobBOSS?**
A: Navigate to the Purchasing module. Locate the open Purchase Order for the material being received. Select the PO and use the receive function to record the received quantity. The system will update the on-hand inventory quantity at the designated bin location. If lot control is enabled on the part, you will be prompted to assign a lot number at the time of receipt. [CONFIRMED]

Detailed coverage: Section 4 (Purchasing and Material Management) [SPECULATIVE — section numbers for earlier chapters not confirmed in this document]

---

**Q: How do I set up material control from scratch?**
A: Follow this sequence before entering any transactions: [CONFIRMED]

1. **Part Master:** Create or verify part records for all stocked materials, including unit of measure, description, and costing method selection.
2. **Bin Locations:** Create bin location records in Table Maintenance and assign at least one bin location to each stocked part.
3. **Costing Method:** Select the costing method (FIFO, LIFO, Standard, or Average) on each part record. This decision is permanent once transactions exist — involve your accountant.
4. **Lot Control:** Enable lot control on any parts that require traceability (heat numbers, serialized material, etc.) before the first receipt.
5. **Reorder Points:** Set reorder point quantities on each stocked part to enable MRP alerts.
6. **First Receipt:** Perform the first receiving transaction for each part to establish starting quantities.

Do not skip or reorder these steps. Each step depends on the previous one being complete.

---

**Q: How do I track heat numbers in JobBOSS?**
A: Heat number tracking uses the lot control system. [CONFIRMED]

1. Enable lot control on the part master record for the material that carries heat numbers.
2. At receiving, when you enter the receipt against the PO, the system will prompt for a lot number. Enter the heat number from the mill certification as the lot number.
3. JobBOSS maintains an audit trail linking the lot number (heat number) to every transaction involving that material — including which jobs it was issued to and what finished parts were produced.
4. The receiving record serves as the permanent link between the heat number and the mill certification documentation.

Note: Lot control must be enabled before the first receipt. Heat numbers received before lot control was enabled will not have lot tracking and cannot be retroactively assigned.

---

**Q: Why are my material costs wrong?**
A: Material cost errors in JobBOSS typically trace to one of three root causes: [CONFIRMED]

1. **Wrong costing method:** Verify the costing method selected on the part master (FIFO/LIFO/Standard/Average). If it was set incorrectly or changed after transactions existed, costs will be inaccurate. This is difficult to correct retroactively.

2. **Lot control not enabled before receiving:** If lot control was enabled after material had already been received, the system may be applying costs inconsistently across lot-tracked and non-lot-tracked inventory.

3. **Bin location costs:** Verify that the bin location assigned to the part has the correct cost basis. Check the bin detail record for the current costed quantity.

If all three are correct and costs are still wrong, pull the Transaction Register Report for the affected part and trace each transaction individually to identify where the cost discrepancy originated.

---

**Q: How do I run an MRP / material requirements planning in JobBOSS?**
A: JobBOSS MRP is accessed through the Quick View Material Forecast Utility. [CONFIRMED]

1. Navigate to the Inventory module.
2. Open the Quick View Material Forecast Utility.
3. The screen displays all parts that are at or below their reorder point or that have demand from open jobs that exceeds available inventory.
4. Review each flagged item. The utility shows current on-hand quantity, demand quantity, and the calculated shortage.
5. For items that need purchasing, the utility supports automatic PO generation — select the items and generate purchase orders directly from the screen.
6. Review and confirm the generated POs before sending to vendors.

Prerequisites for effective MRP: accurate on-hand quantities, reorder points set on all relevant parts, and open jobs with material requirements entered.

---

#### 13.3 Database and Reporting

**Q: How do I connect to the JobBOSS database?**
A: JobBOSS data is stored in Microsoft SQL Server. Connect using ODBC. [CONFIRMED]

1. Install the SQL Server ODBC driver on the client machine if not already present.
2. Open the ODBC Data Source Administrator (32-bit or 64-bit depending on your reporting tool).
3. Create a new System DSN using the SQL Server driver.
4. Enter the SQL Server name or IP address, select SQL Server authentication, and enter valid SQL Server credentials.
5. Select the JobBOSS database name.
6. Test the connection.

Once the ODBC DSN is established, it can be used by Crystal Reports (via Database Expert > ODBC), Microsoft Excel (via Data > Get Data > From ODBC), or any other ODBC-compatible tool.

Note: Use SQL Server credentials with read-only permissions for reporting connections to prevent accidental data modification. [LIKELY — read-only credentials recommendation]

---

**Q: How do I build a custom report for JobBOSS?**
A: Custom reports are built using Crystal Reports connected to the JobBOSS SQL Server database. [CONFIRMED]

1. Establish an ODBC connection to the JobBOSS SQL Server database (see ODBC question above).
2. Open Crystal Reports and create a new report.
3. In the Database Expert, add the ODBC data source and select the relevant JobBOSS tables.
4. Design the report layout — add fields from the selected tables, apply filters (especially date range filters for performance), and format as needed.
5. Save the completed report (.rpt file).
6. To make the report available within JobBOSS, save it to the User Defined reports section in the JobBOSS reports directory.
7. Test the report after any JobBOSS upgrade — table and field names may change, breaking field references.

For Excel-based reporting, use an ODBC connection from Excel (Data > Get Data > From ODBC) to query the JobBOSS database directly. This approach does not require a Crystal Reports license.

---

#### 13.4 Data Collection and Shop Floor

**Q: How do I set up data collection terminals in JobBOSS?**
A: Data collection terminal setup involves both software configuration and hardware deployment. [CONFIRMED]

Software configuration steps:
1. Navigate to Table Maintenance within JobBOSS.
2. Locate the Collection Terminals section.
3. Create a new Collection Terminal record for each physical terminal device.
4. Configure defaults for each terminal: default work center, operator ID prompting behavior, job number format requirements, and any other terminal-specific settings.

Hardware deployment considerations:
- Terminals require network connectivity to the JobBOSS server.
- Job numbers used with terminals must be numeric only — alpha characters in job numbers are not compatible with terminal data entry.
- Train operators on the setup vs. run time entry distinction before the terminal goes live.

Test data collection with a live job before declaring the terminal operational. Verify that time entries appear correctly in JobBOSS job records.

---

#### 13.5 QuickBooks Integration

**Q: How do I sync JobBOSS with QuickBooks?**
A: QuickBooks integration requires careful preparation before the first sync. Follow this sequence: [CONFIRMED]

1. **Ensure unique IDs:** Audit all JobBOSS vendor and customer records. Any company that exists as both a vendor and a customer must have unique IDs for each record. QuickBooks Online does not permit duplicate IDs across vendors and customers.

2. **Install the integration:** Install the JobBOSS-QuickBooks integration component. Confirm whether you are connecting to QuickBooks Desktop or QuickBooks Online — the integration behaves differently for each.

3. **Configure revenue posting:** In the integration settings, set revenue posting to "product code." This is required for financial reports to reconcile between the two systems.

4. **Address mapping:** Verify that each customer's primary address is correctly designated, as QuickBooks Online only supports a single address per customer.

5. **Test sync:** Run a test synchronization and verify that vendors, customers, and transactions transfer correctly before relying on the integration for production accounting.

6. **Periodic journal entries:** Even with correct configuration, periodic journal entries may be required to true up accounts, particularly if revenue posting was incorrect during any period. [CONFIRMED]

---

#### 13.6 Training

**Q: How do I get training for JobBOSS?**
A: Multiple training options are available at different price points: [CONFIRMED]

**Free options:**
- TITANS of CNC Academy at academy.titansofcnc.com — free structured courses including Order Entry, Quoting, Material Planning, Data Collection, and Scheduling with JobBOSS
- ECI video library at resource.ecisolutions.com — 100+ videos, 3-15 minutes each, on-demand
- Free recorded webinar sessions: JobBOSS Dashboard Tips and Tricks (Sessions 1 and 2), Material and Scheduling Webinar

**Paid options:**
- Elite Learning Pass: $29 per month per JobBOSS seat — includes live monthly sessions and 24/7 recorded session access covering all major modules
- Premium Bundle: $79 per month — includes all Learning Passes plus Regional Workshops
- Deep Dive classes: topic-specific intensive sessions available through the training portal

**Contact:** (866) 342-8392 or customerportal.ecisolutions.com/s/training-jobboss

---

#### 13.7 Implementation and Configuration

**Q: What are the most common JobBOSS implementation mistakes?**
A: The following mistakes are most commonly cited as causes of implementation failure or poor ROI: [CONFIRMED]

1. **No dedicated system administrator.** Designate a specific person to own JobBOSS before go-live. This is the single most cited implementation failure factor.

2. **Wrong costing method.** Select FIFO, LIFO, Standard, or Average Cost before the first receiving transaction. This decision is very difficult to reverse.

3. **Skipping data migration.** Enter open POs and establish inventory balances before going live. Skipping this leaves the system with an inaccurate picture of shop status from day one.

4. **Bin locations not created before receiving.** Create and assign bin locations before any receiving transactions.

5. **Lot control not enabled before receiving.** Enable lot control on parts requiring traceability before the first receipt.

6. **Alpha job numbers with data collection terminals.** Use numeric-only job numbers if data collection terminals are deployed.

7. **Not using MRP.** Incorporate the Quick View Material Forecast Utility into the purchasing routine to prevent stockouts.

---

**Q: What should I do if JobBOSS is running slowly?**
A: Slow performance typically has one or more of the following causes, each with a specific remedy: [CONFIRMED]

1. **Database maintenance not performed:** Run SQL Server index rebuilds and statistics updates. Establish a recurring maintenance plan.

2. **Crystal Reports not optimized:** Add date range parameters to reports that scan large transaction tables.

3. **SQL Server upgrade compatibility:** If a recent SQL Server upgrade preceded the slowdown, verify that the installed JobBOSS version is compatible with the new SQL Server version.

4. **Hardware resources:** Verify that the SQL Server host has adequate RAM and CPU for the number of concurrent users.

Contact ECI support at (866) 342-8392 for performance issues that persist after the above steps are completed.

---

#### 13.8 Confidence and Verification Notes

The following items in this manual are marked UNVERIFIED or SPECULATIVE and should be confirmed with ECI Solutions or current product documentation before being relied upon for configuration decisions:

| Item | Section | Status | Recommended Action |
|---|---|---|---|
| ShopBOSS exact functional scope | 10.2, 12 | UNVERIFIED | Contact ECI account representative |
| Full table of contents of Mastering JobBoss book | 10.6 | UNVERIFIED | Review on Amazon before purchasing |
| Names of second and third Learning Passes | 10.3.2 | UNVERIFIED | Check customerportal.ecisolutions.com |
| Individual registration pricing for Regional Workshops | 10.5 | UNVERIFIED | Contact ECI at (866) 342-8392 |
| SQL Server 2017 compatibility for current version | 11.4 | CONFIRMED — verify with ECI before upgrading | Contact ECI support before any SQL Server upgrade |
| uniPoint NC and CAPA feature scope | 12 | LIKELY | Contact uniPoint or ECI for current module documentation |
| EOQ field existence on part master | 11.2 | LIKELY | Verify in JobBOSS Table Maintenance |
| Whiteboard Scheduler vs. Planning Board differentiation | 12 | LIKELY | Review in-application help documentation |

---

This completes Sections 10 through 13 of the JobBOSS/JobBOSS2 Master Reference Manual.


---

Now I have extensive research gathered. Let me compile this into the comprehensive reference manual document.

---

# JobBoss / JobBOSS² User Review Reference Manual
## Synthesized from Capterra, G2, GetApp, TrustRadius, SoftwareAdvice, SelectHub, TEC, SoftwareConnect, Practical Machinist Forums, and More
### Compiled: February 2026 | Based on 1,000+ user reviews across platforms

---

## TABLE OF CONTENTS

1. Executive Summary & Platform Ratings
2. Product History & Background
3. Overall User Sentiment by Platform
4. Pros: Comprehensive Breakdown with Frequency Data
5. Cons: Comprehensive Breakdown with Frequency Data
6. Module-by-Module User Feedback
   - 6.1 Estimating & Quoting
   - 6.2 Order Processing & Job Management
   - 6.3 Material Control & Inventory Management
   - 6.4 Shop Floor Data Collection
   - 6.5 Scheduling & Capacity Planning
   - 6.6 Purchasing Module
   - 6.7 Accounting & Financial Management
   - 6.8 QuickBooks Integration
   - 6.9 Reporting (Crystal Reports & Stimulsoft)
   - 6.10 Quality Control & Compliance (AS9100/ISO)
   - 6.11 Mobile Apps & DCMobile
7. Known Technical Limitations & Field Constraints
8. Performance & Database Issues
9. Customer Support & Implementation Feedback
10. Pricing & Cost Concerns
11. User Workarounds & Tips
12. JobBOSS Ideas Portal: Top Feature Requests
13. Competitive Comparisons
    - 13.1 JobBOSS vs E2 Shop System
    - 13.2 JobBOSS vs ProShop ERP
    - 13.3 JobBOSS vs Other Alternatives
14. User Quotes from Real Reviews
15. Who Is This Software Best / Worst For
16. Summary Verdict

---

## 1. EXECUTIVE SUMMARY & PLATFORM RATINGS

JobBOSS / JobBOSS² is a manufacturing ERP and shop management system developed originally for job shops, now owned and operated by ECI Software Solutions following their 2017 acquisition of the original JobBOSS and their 2020 acquisition of Shoptech Industrial Software (the maker of E2 Shop System). The two products were subsequently merged and rebranded as JobBOSS².

### Aggregate Ratings Across Platforms (as of 2025–2026)

| Platform | Rating | Review Count | Notes |
|---|---|---|---|
| Capterra | 4.2 / 5.0 | ~865 reviews | Verified users |
| G2 | ~4.0 / 5.0 | 50+ reviews | Verified purchasers |
| GetApp | ~4.2 / 5.0 | 110,000+ aggregate | Mirrors Capterra |
| TrustRadius (JobBOSS original) | 7.2 / 10 | 15 reviews | |
| TrustRadius (JobBOSS²) | 8.0 / 10 | 61 reviews | Top Rated badge |
| SoftwareAdvice | ~4.2 / 5.0 | 865+ reviews | Mirrors Capterra |
| SelectHub User Satisfaction | 83% | 1,012 reviews | "Great" tier |
| TEC (Technology Evaluation) | Positive | Dozens | Expert + user |
| SoftwareConnect | Positive | Curated | |
| Practical Machinist Forum | Mixed | 100+ threads | Raw user community |

**Overall consensus:** JobBOSS² receives strong marks from small-to-mid-sized job shops and make-to-order manufacturers. The satisfaction gap between the legacy on-premise JobBOSS (7.2 TrustRadius) and the newer JobBOSS² cloud product (8.0 TrustRadius) is notable, suggesting users find real improvement in the newer product — yet persistent frustrations with reporting, support, pricing, and field-length limitations remain.

---

## 2. PRODUCT HISTORY & BACKGROUND

- **Original JobBOSS**: Developed for small-to-medium job shop manufacturers; gained large installed base across North America.
- **E2 Shop System** (by Shoptech): A direct competitor, also targeting job shops; known for its intuitive UI and cloud-native architecture.
- **2017**: ECI Software Solutions acquires JobBOSS.
- **2020**: ECI acquires Shoptech Industrial Software (E2 Shop System developer), citing E2's intuitive UI and cloud-native capabilities.
- **Post-2020**: ECI merges the two products' best features into **JobBOSS²**, which is the current primary product.
- **Current ownership**: ECI Software Solutions, a private equity-backed company with multiple manufacturing software brands.

### What Changed with ECI Ownership (User Perspective)
- Pricing increased substantially: one forum user noted seat costs went from approximately $1,200/seat (2012 era) to $3,600/seat.
- One user reported costs escalating from $10,000 to "well over $40,000" following ECI's acquisition, while certain modules like purchasing and scheduling still didn't work properly.
- Users characterize ECI as "only about money," with complaints that support and feature development speed decreased after acquisition.
- ECI does not publish pricing publicly, which users flag as a frustration point.

---

## 3. OVERALL USER SENTIMENT BY PLATFORM

### Capterra (865 Reviews — Most Authoritative Dataset)

The modal review on Capterra is a 4-star or 5-star rating from a small or mid-sized job shop owner, operations manager, or shop floor supervisor. The most common profile of a satisfied user is someone running 1–50 employees who does custom/make-to-order manufacturing and uses JobBOSS as their primary system for tracking jobs from quote to invoice.

The modal negative review tends to come from users who either: (a) outgrew the system and found it too rigid, (b) experienced poor implementation support, (c) found the reporting insufficiently flexible, or (d) have complex scheduling needs that the system doesn't accommodate.

**Capterra Top-Line Summary:**
- Value for Money: Above average for the market segment
- Ease of Use: Generally positive, with some outliers who find legacy UI difficult
- Customer Support: Split — some users praise it highly, others report 3-week response delays
- Features: Strong for core job shop operations, weak for advanced analytics and complex scheduling

### G2 (JobBOSS² Listing)

G2 reviews tend toward the more technical. Users here include IT admins, ERP managers, and power users who interact deeply with integrations and customization. Complaints on G2 skew toward:
- QuickBooks integration being "wonky"
- Pricing perceived as unfairly high for small businesses
- Complexity of tailoring the system to specific industry sub-niches

Praise on G2 includes:
- Efficient inventory management
- Logical workflow for job shop environments
- Scalability from micro-business upward

### TrustRadius (JobBOSS² — 8.0/10, Top Rated)

TrustRadius reviewers are generally mid-market professionals. Key patterns:
- **Project Planning and Scheduling** is the highest-rated function (score of 10 from one set of reviewers)
- Multi-department use is praised: sales, accounting, shipping, purchasing all rely on it daily
- QuickBooks integration earns praise for reducing data entry in accounting
- Customization "can prove to be somewhat of a challenge to implement, but support always jumps in to assist"
- Packing slips are called out as "cumbersome"

### Practical Machinist Forums (Raw Community Sentiment — Most Critical)

The Practical Machinist forum represents the least-filtered user feedback. Recurring themes in multiple threads:

- Users frustrated with the software feeling "clunky"
- Implementation quality described as inconsistent
- Scheduling module repeatedly criticized for multi-machine and multi-pallet scenarios
- Price increases post-ECI acquisition mentioned repeatedly as deal-breakers
- Multiple threads with users asking "Is there anything better?" and finding limited alternatives at similar price points
- Users who switched to ProShop or other alternatives often report significant improvements in usability and on-time delivery

---

## 4. PROS: COMPREHENSIVE BREAKDOWN WITH FREQUENCY DATA

The following pros are ranked by how frequently they appear across review platforms:

### PRO #1: End-to-End Workflow Integration
**Frequency: Very High — mentioned in majority of positive reviews across all platforms**

The single most praised feature is how JobBOSS flows the entire manufacturing process from start to finish. Users repeatedly describe the logical progression:
- Estimating → Quote → Sales Order → Work Order/Job → Shop Floor → Shipping → Invoicing → Accounting

Reviewers describe this as the "best feature" and a significant competitive advantage for job shops. The system keeps all data linked, so when a quote converts to an order, all cost information carries forward. When a job ships, it can trigger invoicing. This end-to-end traceability is what keeps long-term users loyal despite other frustrations.

**Representative sentiment:** "The best feature is how it flows through the system — from estimating to actually creating an order to shipping to receiving to billing. The workflow is very easy to understand."

### PRO #2: Job Tracking & Real-Time Shop Visibility
**Frequency: High — mentioned in a large plurality of reviews**

Users value the ability to see where every job is at any moment:
- Which workstation is processing which job
- Which jobs are ahead of or behind schedule
- Real-time status from work center updates
- Identifying bottlenecks by machine, employee, week, day, or hour

Production managers, shop owners, and schedulers consistently list this as a reason they stay with the product. The Planning Board and Whiteboard Scheduler allow drag-and-drop adjustments with immediate visual feedback.

### PRO #3: Job Costing Accuracy
**Frequency: High**

Users consistently note that JobBOSS provides meaningful insight into actual vs. estimated performance on a per-job basis:
- Labor hours estimated vs. actual
- Material costs estimated vs. actual
- Overhead allocation
- Profitability by job, customer, or part number

For job shops that previously managed this on spreadsheets or whiteboards, the improvement is described as transformational. One user called it "a significant upgrade in terms of job costing accuracy."

### PRO #4: Ease of Onboarding New Users
**Frequency: High**

Many reviewers highlight that the system is easy to bring new employees onto quickly. The interface logic matches the job shop mental model (quote → job → ship → bill), which makes it intuitive for shop-floor people who may not have ERP experience. 

**Caveat noted:** This praise applies primarily to the JobBOSS² version. Legacy on-premise users are more divided, with some calling it "clunky" and difficult for new users.

### PRO #5: Suitability for Small Manufacturers
**Frequency: High**

Reviewers consistently note that JobBOSS is designed specifically for the small-to-medium job shop segment, unlike generic ERPs that require extensive customization to handle job shop workflows. Users in the 1–50 employee range frequently report:
- Increased throughput (one user cited 20%+ increase)
- Reduced paperwork
- Better scheduling discipline
- Improved on-time delivery

### PRO #6: Estimating Module
**Frequency: Moderate-High**

The estimating module earns consistent praise:
- Create detailed cost estimates for labor and materials
- Store templates for repeat-style jobs
- One-click conversion from quote to job order
- AI-powered BOM builder (JobBOSS² feature) that generates draft bills of materials from PDFs, Excel files, images, and CSVs
- Historical part data available to improve future estimates
- Estimated vs. actual reporting for continuous improvement

### PRO #7: Affordability (Relative)
**Frequency: Moderate — especially in pre-ECI-era reviews; declining post-acquisition**

Earlier reviews frequently praised affordability. Post-ECI acquisition reviews are more mixed. For new users coming from larger ERPs (SAP, NetSuite), the price still appears competitive. For existing users who have seen prices increase, this praise has diminished.

### PRO #8: Customer Support Quality (Variability High — See Also Con #1)
**Frequency: Moderate — strongly contradicted by a significant minority**

When support works well, users are highly satisfied:
- "Fast, knowledgeable, and friendly support staff"
- "Support always jumps in to assist"
- Users note the knowledge base and training videos are helpful

This is one of the most contested areas — see Con #1 for the opposing view.

### PRO #9: Inventory Management & Material Tracking
**Frequency: Moderate**

The materials module is praised for:
- LIFO, FIFO, Standard, and Average Cost tracking
- Multiple bin locations with costs at bin level
- Reorder levels and automated purchasing triggers
- Just-in-time purchasing tied to job requirements
- Material history showing cost fluctuations over time
- Where raw materials were pulled from when allocated to jobs

### PRO #10: Scheduling Visibility (When Working)
**Frequency: Moderate — but heavily caveated**

When scheduling is functioning in straightforward scenarios (single machine per job, standard job types), users find the finite/infinite scheduling toggle, Gantt charts, and drag-and-drop planning board valuable. This praise is heavily offset by the scheduling cons documented below.

### PRO #11: Templates & Customization for Specific Needs
**Frequency: Moderate**

Aerospace and defense manufacturers who need AS9100/ISO certification support find the template system valuable. Users report creating and storing custom templates for certification needs, printing numerous custom reports, and leveraging the uniPoint quality integration effectively.

### PRO #12: Multi-Department Utility
**Frequency: Moderate**

Unlike some point solutions, JobBOSS serves multiple departments:
- Sales (quoting, customer records)
- Production (job tracking, scheduling)
- Purchasing (POs, vendor management)
- Accounting (A/R, A/P, G/L, payroll)
- Shipping (packing lists, shipment records)
- Quality (inspection, certifications)

This breadth means a single system can run a small shop without needing separate tools for each function.

---

## 5. CONS: COMPREHENSIVE BREAKDOWN WITH FREQUENCY DATA

### CON #1: Customer Support & Response Time
**Frequency: Very High — among the most commonly cited complaints**

This is the single most consistent and severe complaint across all platforms. Multiple documented examples:
- Initial response to a support ticket taking **over 3 weeks**
- A system-caused large inventory error: initial response with repeat follow-up took 3 weeks, and the user was still reaching out to others after 4 weeks
- Custom report requests taking weeks for feedback
- One user described it as "the worst implementation in my 45 years of implementing manufacturing software systems" — their customer service rep was "late or non-responsive"
- ECI does not offer 24/7 phone support — hours are 7am–7pm CST Monday through Friday only
- Users experiencing issues during off-hours or weekends must wait for assistance
- Implementation people are described as generally good, but post-implementation support quality is inconsistent

**Pattern:** Support quality appears to depend heavily on which representative a user is assigned. Power users and early adopters report much more positive support experiences than new customers or those who purchased after the ECI acquisition.

### CON #2: Reporting Limitations
**Frequency: Very High — second most commonly cited complaint**

Reporting is a persistent pain point:
- Default reports lack flexibility for grouping, filtering, or drilling down in ways users need
- Users cannot easily create reports that span multiple jobs with similar materials or customers
- The built-in report editor is insufficient — users must purchase and learn Crystal Reports or pay ECI for custom report development
- JobBOSS² uses **Stimulsoft** as its report designer, but Stimulsoft cannot provide support to JobBOSS² users (only to holders of a direct Stimulsoft license) — creating a support gap
- Crystal Reports custom work can be extremely slow — a documented case shows a custom report taking **four minutes to generate** when not optimized properly
- The root cause of Crystal Reports slowness: using multiple Command objects instead of a single unified query causes Crystal to pull all data into memory and join it there rather than pushing the join to the database
- Profit and loss reports are described as "difficult to interpret" with "numbers that don't make sense"
- Advanced analytics require external customization
- Requesting custom reports from ECI takes weeks and costs extra money

**Common workaround:** Users build reports in Crystal Reports by writing a single SQL Command that pulls all required data in one query, with WHERE clauses to filter at the database level rather than in memory.

### CON #3: Scheduling Limitations for Complex Scenarios
**Frequency: High**

While basic scheduling is functional, the system struggles with:
- **Multi-machine / multi-pallet jobs**: Users report having to invent their own workarounds for scheduling jobs that run on multiple machines simultaneously
- **Split jobs and blanket orders**: Scheduling can get complex; users recommend using the Job Split function or the Sales Order module for blanket orders with multiple release dates
- **Backward scheduling default**: The system defaults to backward scheduling (working back from promise date to set start date), which doesn't always match how shops think forward
- **Limited capacity simulation**: Some reviewers feel the scheduling tool "doesn't come with more features" and is lacking compared to dedicated scheduling tools
- **Difficulty figuring out**: Scheduling is described as "difficult to figure out" by multiple reviewers

**User tip documented in reviews:** When dealing with blanket orders, enter the order with one job number and use the "Job Split" function to enter release dates and quantities, creating sub-job numbers (R1, R2, etc.) for each release.

### CON #4: Pricing & Cost Increases
**Frequency: High — especially post-ECI acquisition**

Pricing complaints fall into several categories:
- **Per-seat pricing**: Starting at approximately $200/user/month for JobBOSS²; implementation typically starts at $5,000
- **Legacy price increases**: Users who have been on the platform for years report dramatic cost increases: $1,200/seat in 2012 to $3,600/seat in later years
- **Total cost escalation**: One user reported costs going from $10,000 to "well over $40,000" while key modules (purchasing, scheduling) still didn't work
- **Hidden costs**: Customization, custom reports, additional modules, and support packages incur significant additional charges
- **Subscription lock-in**: Users who tried to leave were reportedly told to organize everything in one week and then had their subscription shut off until paying for a year contract
- **Small business burden**: The pricing structure is described as "confusing or burdensome for smaller companies," especially when certain features require extra payments

### CON #5: Software Rigidity / Inflexibility
**Frequency: High**

- The system is frequently described as not adapting well to unusual workflows or specialized industry niches within manufacturing
- Users who outgrow the system's standard assumptions find it forces them to build workarounds
- One user specifically stated: "It has been the worst implementation in my 45 years of implementing manufacturing software systems. Our customer service rep is late or non-responsive... Software is very limited on flexibility adapting to a company's industry needs."
- The program is described by some users as "rigid and not built for custom machine shops, despite being advertised as customizable and flexible"
- Creating a packing list requires 4 steps, and once made the job is closed, requiring 4 steps to undo and 4 more to redo

### CON #6: Field Length Restrictions / Character Limits
**Frequency: Moderate-High — concentrated among manufacturing users with complex part numbering**

This is a technical issue that causes real operational pain, particularly for aerospace, defense, and automotive suppliers:

| Field | Limit | Problem |
|---|---|---|
| Part Number / Material ID | 30 characters displayed (50 can be typed but only 30 show on PO) | Cannot fit manufacturer name + part number together |
| Material Description | 30 characters | Items end up with identical truncated descriptions |
| Drawing Number | 16 characters | Insufficient for aerospace part numbers |
| Job Number field | Limited | Users request expansion |
| Revision Field | 8 characters | Aerospace ADCN/revision numbers frequently exceed this |

**User quotes on this issue:**
- "The 30 character material limit is often too short to fit the manufacturer and their part number together without losing important part data."
- "Users end up with a significant number of items with the exact same description since they get cut off after 30 characters — fabrication and integration shops are driven crazy by it."
- "When you include the Parts List or ADCN it will quickly exceed 8 characters."

**Active Ideas Portal requests**: Multiple feature requests on ideas.jobboss.com for expanding character limits — including JBCORE-I-81 (Material IDs), JBCORE-I-290 (Material Description), JBCORE-I-377 (Job Number), JBCORE-I-1283 (Revision field), and JBCORE-I-1234 (general ability to modify field sizes).

### CON #7: Interface / UI Feels Dated
**Frequency: Moderate-High**

- Multiple reviewers note the interface feels "outdated compared to modern cloud-based ERP solutions"
- Described by some as resembling "older DOS-based systems"
- "Conflicting visual cues which could initially confuse new users"
- Legacy on-premise version significantly more criticized than the newer JobBOSS² web interface
- Users who switch to ProShop or other modern ERPs consistently comment on the contrast in UI modernity

### CON #8: Scalability Ceiling
**Frequency: Moderate**

- "There seems to be a ceiling on scalability with the number of parts" — one user noted problems managing products with 600–700 components
- The system is optimized for small-to-mid manufacturers; users with 50+ employees and complex multi-level assemblies begin hitting limits
- Integration with modern tooling (CAD systems like Paperless Parts) is described as difficult: "struggling to get data points from Paperless Parts into the system"

### CON #9: QuickBooks Integration Issues
**Frequency: Moderate**

Documented problems with the QuickBooks integration:
- "The Accounting can be a little wonky sometimes in how it integrates with QuickBooks" — G2 review
- Sales Tax and Payment Terms are NOT passed from JobBOSS to QuickBooks — requires manual editing in QuickBooks for every new customer
- Payment terms settings do not transfer even when creating customers in QuickBooks first and syncing to JobBOSS
- Vendor cash discounts are not supported in the QuickBooks Online version of the integration
- Vendor IDs and Customer IDs must be unique — may require a one-time database cleanup before transitioning
- The integration is described as requiring "15 plus steps" by one SoftwareConnect reviewer
- Some users recommend shifting all payables tracking to QuickBooks Online and using JobBOSS² only for operations, to reduce integration friction

### CON #10: Mobile App Reliability
**Frequency: Moderate**

The Employee DC App (DCMobile) has documented recurring issues:
- **Document viewing crashes**: App closes when users try to view documents attached to a job on iPhone — reportedly works only "30% of the time"
- **Camera functionality**: A rebranding update broke camera functionality
- **Login failures**: Updates have broken login for some users who depend on the app daily; support cases left unresolved for over a month
- **Scanner defaults**: No way to set a default action after scanning — constantly requires user confirmation
- **Attachment viewing**: Feature requested 2+ years ago (DCMOBILE-I-10) still not implemented
- **System glitches**: Recurring crashes and slow response times affecting productivity

### CON #11: Learning Curve & Training Quality
**Frequency: Moderate**

- "The training included with JobBOSS is really bad, resulting in companies paying for a program before they are able to use it"
- Implementation quality is inconsistent — some users receive excellent service, others receive minimal guidance
- Some companies resorted to working with third-party developers rather than using ECI's implementation team
- Some customers were sent the wrong training course
- The software is described as requiring dedicated internal trainers/champions to drive adoption

### CON #12: System Performance / Slowness
**Frequency: Moderate — but significant when encountered**

- The program is described in community reviews as "very slow, so much so that users click an icon again without realizing their first request is still being processed"
- Crystal Reports performance issues: reports that pull multiple data sets in separate commands instead of a single query are dramatically slower
- One documented case of a Crystal Report taking **4 minutes** to generate, resolved by rewriting as a single SQL command
- The root performance issue is architectural: multiple Command objects in Crystal Reports force in-memory joining rather than database-level joining

---

## 6. MODULE-BY-MODULE USER FEEDBACK

### 6.1 Estimating & Quoting

**Overall User Rating: Strong (one of the highest-rated modules)**

**What Works Well:**
- Creating detailed cost estimates with labor and material breakdown
- Template storage for repeat-style jobs — users can copy previous quotes and adjust
- One-click quote-to-job-order conversion
- Maintaining part history for repeat parts to speed future quotes
- Estimated vs. actual comparison reporting to improve future quotes
- AI BOM builder in JobBOSS² that generates draft bills of materials from PDFs, Excel files, images, and CSVs
- Multiple review stages: draft estimates can be reviewed, edited, then approved

**What Users Complain About:**
- Limited ability to model complex multi-level assemblies with hundreds of components
- The quote conversion workflow can be cumbersome for very high-volume quote environments
- Some users find template management requires discipline to keep current — stale templates lead to inaccurate estimates

**Tips from Users:**
- Build out complete material and labor templates for your most common job types early in implementation — this pays dividends for every future quote
- Use the estimated vs. actual reports regularly to identify where your estimates are consistently off
- The AI BOM builder is best treated as a starting draft — always review before converting to an order

---

### 6.2 Order Processing & Job Management

**Overall User Rating: Strong**

**What Works Well:**
- The sales order → work order → shop traveler flow is logical and well-designed for job shops
- Job status tracking across all open orders in real time
- Support for multiple job types: single-level, multi-level assembly, blanket orders, split jobs, time-and-material jobs
- Sub-jobs, split jobs, and job revisions can be tracked
- Customers can have individual pricing and terms stored at the customer level
- Job travelers can be printed with routing steps, material requirements, and quality checkpoints

**What Users Complain About:**
- Creating a packing list requires 4 steps, and if you make an error, undoing and redoing requires another 8 steps total
- Packing slips are described as "cumbersome" on TrustRadius
- Blanket orders with multiple delivery dates require users to implement the Job Split workaround
- Field length restrictions (see Section 7) affect job numbers, part numbers, and descriptions

**Tips from Users:**
- For blanket orders: enter the master order with a base job number, then use Job Split to create sub-jobs (e.g., J1234-R1, J1234-R2) for each release date and quantity
- Use the Sales Order module for blanket orders — it makes managing multiple release dates significantly easier than trying to schedule the entire blanket order quantity at once

---

### 6.3 Material Control & Inventory Management

**Overall User Rating: Moderate — strong concept, execution issues**

**What Works Well:**
- The concept of tying material supply to job demand is well-designed
- LIFO, FIFO, Standard, and Average Cost tracking methods all available
- Multiple bin locations with bin-level cost tracking
- Reorder levels with automated purchase triggers
- Material history showing cost fluctuations over time
- Tracking where raw materials were pulled from when allocated to specific jobs
- Vendor Owned Inventory Utility for consignment stock scenarios
- Physical Inventory module for periodic counts
- Material Selling Price Update utility for bulk price adjustments
- Material Cost Update utility

**What Users Complain About:**

**The Core Problem:** The materials module requires consistent discipline from all users. If receiving, issuing, and return processes are not followed by everyone, every time, the inventory data becomes unreliable. This is reported as a major source of frustration.

Specific issues documented:
- "If they're not going to be tracking what comes in and is used, the materials module just isn't practical — it requires receiving items with data collection and issuing quantity to each job"
- Identifying which user made a specific material transaction is difficult with 100+ users — the system has a Material Transaction User Tracking Utility to address this, but it adds administrative overhead
- Transfers can be traced via barcode scanners and DCMobile, but PC-based transfers are harder to attribute to specific users
- The inventory module in the web version is described as "not very intuitive or conducive for precise inventory control"
- A system-caused "large inventory error" was cited in one Capterra review, with the response time from support being 3+ weeks
- Inventory Transfer Traceability was requested as a feature (JBCORE-I-483) and merged into the broader JobBoss History request (JBCORE-I-303)

**Tips from Users:**
- Establish written SOPs for receiving, issuing, and returning materials before going live
- Use barcode scanning for all material transactions — it provides better audit trails than keyboard entry
- Run the Material Transaction User Tracking Utility regularly to identify users who are skipping steps or entering incorrect quantities
- Use the Physical Inventory module on a scheduled cycle-count basis rather than relying solely on perpetual inventory
- The Vendor Owned Inventory Utility is useful for consignment scenarios but requires vendor cooperation on reporting

---

### 6.4 Shop Floor Data Collection

**Overall User Rating: Moderate — valuable concept, adoption challenges**

**What Works Well:**
- Barcode scanning for clocking in and out of jobs
- Running multiple machine clock-ins simultaneously without conflicts
- Touch-screen kiosks and tablet-based data entry for shop floor workers who are not office users
- Integration with DCMobile app for mobile data capture
- Keyboard-wedge barcode scanners work natively with the system (treated as keyboard input)
- All clock-in data feeds directly into job costing, providing real labor actuals vs. estimates

**What Users Complain About:**

**The Adoption Problem:**
- Workers who realize they're being tracked develop defense mechanisms — including scanning out early to appear as fewer hours on record
- Excessive scanning requirements slow down actual work
- Having only one computer per building makes data entry a bottleneck
- "The extra scanning and paperwork can actually impede workflow" — Practical Machinist forum

**Hardware Issues:**
- Inexpensive Amazon barcode scanners work fine for 1D/2D barcodes (confirmed by community users)
- Refurbished Motorola Symbol scanners ($20 on eBay) handle 2D barcodes adequately for most needs
- No ability to set a default scanner action — every scan requires confirmation (see Ideas Portal DCMOBILE-I-1)

**Mobile App Problems (DCMobile):**
- App crashes when viewing job attachments on iPhone (works ~30% of the time)
- Recent updates broke camera functionality
- Recent updates broke login for some users
- Cases open for over a month without resolution

**Tips from Users:**
- Distribute data entry stations generously across the shop floor — one per zone at minimum
- Use barcode labels on job travelers and material bins to minimize manual entry
- Brief shop floor workers on WHY tracking matters (job costing, scheduling accuracy) — this improves buy-in
- Use inexpensive keyboard-wedge scanners rather than full mobile handheld devices where WiFi coverage is uncertain
- If using DCMobile on tablets, test document viewing thoroughly before relying on it in production

---

### 6.5 Scheduling & Capacity Planning

**Overall User Rating: Moderate — praised for visibility, criticized for complexity**

**What Works Well:**
- Finite and infinite scheduling modes available
- Planning Board with drag-and-drop functionality
- Whiteboard Scheduler for real-time adjustments to customer due dates and job schedules
- Gantt chart view for production schedule visualization
- Capacity view by machine, employee, month, week, day, or hour
- Integration with job costing so schedule changes update cost projections
- Backward scheduling from promise date to calculate start dates
- Visual identification of machines that are occupied, idle, or needed for in-progress work

**What Users Complain About:**

**The Multi-Machine Problem:**
"If you run multiple jobs on a single machine or use multi-pallet machines, scheduling can get frustrating, and you will have to come up with your own solutions to schedule those machines." — Capterra reviewer

**Split Job Complexity:**
Blanket orders with multiple delivery dates require the Job Split workaround to get the schedule to work correctly against release quantities rather than total order quantities.

**Backward Scheduling Default:**
The system's preference for backward scheduling (calculating start dates by subtracting time from the due date) doesn't match how many shops think about capacity forward from today.

**Learning Curve:**
"Scheduling can be difficult to figure out" — consistent across multiple reviews.

**Limited Advanced Features:**
- Reviewers felt the scheduling tool "didn't come with more features"
- No built-in simulation capability for what-if capacity scenarios
- Limited integration with external advanced scheduling tools

**Community Forum Observations (Practical Machinist):**
- Multiple threads indicate users either love or hate the scheduling module
- Long-time users who have adapted their processes to match the system's assumptions are satisfied
- Users who came from more flexible scheduling systems (or manual whiteboard methods) often find it limiting

**Tips from Users:**
- Set up work centers carefully on the front end — work center configuration is foundational to all scheduling accuracy
- Use the finite scheduling mode to get realistic capacity loading; use infinite to understand theoretical earliest completion
- For blanket orders, use Job Split to align schedule to release quantities
- Consider supplementing the built-in scheduler with a dedicated third-party scheduling tool if you run highly complex multi-operation, multi-machine job types

---

### 6.6 Purchasing Module

**Overall User Rating: Moderate-High**

**What Works Well:**
- Supports standard POs, blanket/contract POs, subcontract orders, purchase requests, and returns-from-stock
- RFQ (Request for Quote) workflow from job BOM to vendor selection to PO
- Tracks all vendor-related information: contacts, services offered, terms, lead times
- Historical purchasing data feeds back into job estimating
- Integration with job costing: material receipts update job costs in real time
- Automated reorder and PO generation based on inventory minimums
- Cash requirements reporting for AP

**What Users Complain About:**
- "Somewhat limited on the part variation when it came to vendors" — unable to distinguish new revisions of the same part from the same supplier without entering new part numbers
- The Vendor Reference field conflicts with the Extended Description field on POs: if you enter a value in Vendor Reference or Detail Number, it overwrites the space for Extended Description on the Purchase Order
- The 30-character part number limit creates vendor management challenges (same as the broader field-length issue)
- Integration with QuickBooks Online requires significant manual workarounds for tax, payment terms, and vendor cash discounts

**Tips from Users:**
- Establish a clear part numbering convention before going live — the 30-character limit forces discipline
- Use the Vendor Reference field judiciously; be aware it displaces Extended Description on the PO
- For subcontract operations, ensure the subcontract PO type is used correctly to link back to the parent job routing step

---

### 6.7 Accounting & Financial Management

**Overall User Rating: Moderate — adequate for small shops, limiting for larger ones**

**What Works Well:**
- Standard A/R and A/P modules cover basic needs for small shops
- Automated check writing with cash requirements reporting
- Invoice management tied to purchase orders and job receipts
- G/L integration
- Payroll module available
- CenterPoint Payroll integration available as a third-party option

**What Users Complain About:**
- Many users treat JobBOSS as a manufacturing operations tool and use QuickBooks for all actual accounting — the built-in accounting is considered secondary
- Profit and loss reports described as "difficult to interpret" with numbers that "don't make sense" to users
- The accounting module is described more as "MRP than ERP" by some reviewers
- AR adjustment module has documented issues: one case involved accidentally entering duplicate credits to AR, and the path to resolve it was unclear

**Tips from Users:**
- If you're a QuickBooks user, consider using JobBOSS for operations only and QuickBooks for all financial reporting
- Run AR aging reports regularly and reconcile against QuickBooks if using dual-system approach
- For payroll, consider the CenterPoint Payroll integration for a more robust solution than the built-in payroll module

---

### 6.8 QuickBooks Integration — Detailed Feedback

**Overall User Rating: Low-to-Moderate — frequently described as problematic**

This is one of the most consistently problematic areas identified across review platforms. Below is a comprehensive breakdown of documented issues:

**Version History:**
- JobBOSS QuickBooks Integration has gone through multiple versions (documented up to v5.3 and v5.5)
- More recent JobBOSS² versions integrate with both QuickBooks Desktop and QuickBooks Online
- Chortek Consulting (a third-party partner) has published a multi-part guide on the integration, suggesting it is complex enough to require specialist guidance

**Documented Integration Bugs and Limitations:**

1. **Sales Tax Not Passed**: JobBOSS QuickBooks Integration (as of v5.5) does not pass Sales Tax information from JobBOSS invoices to QuickBooks. Every customer invoice requires manual tax field updates in QuickBooks.

2. **Payment Terms Not Passed**: Payment terms set in JobBOSS are not transferred to QuickBooks, even when customers are created in QuickBooks first and then synced to JobBOSS.

3. **Vendor Cash Discounts**: Not supported in QuickBooks Online version of the integration — known limitation with no automatic resolution.

4. **Unique ID Requirement**: Vendor IDs and Customer IDs must be unique in JobBOSS² — may require a one-time database cleanup before transitioning to QuickBooks Online integration.

5. **Complexity**: One SoftwareConnect reviewer described the integration as requiring "15 plus steps."

6. **"Wonky" Accounting**: G2 review describes the accounting integration as "a little wonky sometimes in how it integrates with QuickBooks" — vague but frequently echoed.

7. **G/L History Conversion**: When transitioning from on-premise to cloud/QuickBooks Online, converting G/L history, customers, vendors, and invoices requires detailed planning and execution.

**User Workarounds:**
- Best practice recommended by Chortek Consulting: shift all payables tracking to QuickBooks Online and use JobBOSS only for operational data, reducing the number of data sync touch points
- Manual entry of tax rates and payment terms for new customers as a standard operating procedure
- Some users maintain dual entry for critical financial data during the first months of integration until they trust the sync

**Who Is Most Affected:**
- Companies with complex sales tax scenarios (multi-state, multiple jurisdictions)
- Companies with vendor cash discount programs
- Companies transitioning from legacy on-premise to JobBOSS² cloud + QuickBooks Online

---

### 6.9 Reporting — Crystal Reports & Stimulsoft

**Overall User Rating: Low — most consistently criticized area alongside support**

Reporting is arguably the most persistent and technically complex complaint area in all JobBOSS reviews.

**Two Reporting Environments:**

**Legacy JobBOSS (On-Premise):** Uses Crystal Reports (SAP Crystal Reports) as the customization layer.

**JobBOSS² (Cloud/Modern):** Uses Stimulsoft as the embedded report designer, with Crystal Reports still available for users who need it.

---

**Crystal Reports Issues (Legacy & Ongoing):**

**Performance Problem — Documented and Solvable:**
A documented SAP Community case shows a custom JobBOSS Crystal Report taking **4 minutes to generate**. Root cause:
- Multiple Command objects in a single report cause Crystal to pull all data into memory and then join it, rather than pushing the join to the database server
- Fix: Rewrite as a single Command (SQL query) with proper WHERE clauses to filter at the database level
- Result: Dramatic performance improvement (from 4 minutes to seconds in the documented case)

**Formula Errors:**
- JobBOSS formulas come from a User Function Library (UFL) provided with the software
- When custom formulas produce errors, users must contact JobBOSS support — Crystal Reports itself cannot assist because the UFL is proprietary to JobBOSS
- This creates a support dependency for every formula-related issue

**Script Error on Custom Reports:**
- A documented issue: when importing Crystal Reports into JobBOSS, users must ensure the UD_Report.htm file is identical in both the "Reporting" and "Reporting\User-Defined" directories
- The Report Name must exactly match the Title in the Document Properties or a script error occurs
- This is a non-obvious technical requirement that trips up many users

**No Built-In Report Editor:**
- Feature request JBCORE-I-1418 on the Ideas Portal specifically requests a "Built-In Report Editor" so users can edit reports from within the software without purchasing Crystal Reports separately
- This is a highly voted request, indicating significant user frustration with the status quo

**Cost of Crystal Reports Customization:**
- ECI charges for custom report development
- Custom report requests take weeks for feedback
- Third-party consultants (e.g., ReportingGuru.com) have built a business specifically around JobBOSS Crystal Reports customization

---

**Stimulsoft Issues (JobBOSS²):**

**Support Gap:**
- Stimulsoft is **unable** to provide support to customers of their clients (i.e., JobBOSS users) who do not hold a direct Stimulsoft license
- Stimulsoft specifically states that developers sometimes restrict or change functionality in their embedded implementations, and in those cases "users will have to send requests to the JobBOSS developers"
- This creates a situation where users hit a wall: ECI is the only entity that can help with Stimulsoft report issues in JobBOSS², and ECI support can be slow

**File Format Compatibility:**
- Reports bundled with JobBOSS² use a .cff file extension
- Users have reported compatibility issues when working with embedded Stimulsoft reports
- Variable comparisons and conditional logic in reports have generated documented errors

**Practical Impact:**
- Many users abandon the Stimulsoft designer and either pay ECI for custom report work or use Crystal Reports instead
- ReportingGuru.com explicitly offers Crystal Reports customization services for JobBOSS, suggesting a commercial market has developed to fill this gap

---

**What Users Want From Reporting (From Review Data):**

1. The ability to group multiple jobs by customer or material for consolidated analysis
2. Profitability dashboards with visual charts and graphs (not just tabular reports)
3. Easy-to-interpret P&L reports
4. A built-in report editor that doesn't require purchasing a separate tool
5. Scheduling dashboards with visual job status indicators
6. Reports that pull from multiple modules without requiring custom SQL
7. KPI dashboards that are configurable by role (owner vs. shop floor vs. accounting)

---

### 6.10 Quality Control & Compliance (AS9100/ISO)

**Overall User Rating: Moderate-High when using uniPoint integration**

**Native Quality Features:**
- Inspection checkpoints can be embedded in job routings
- Quality hold and quarantine functionality via the Quality Control Quarantine Inventory Manager utility
- AS9100 and ISO 9001 compliance supported
- Document management with revision control

**uniPoint Integration (Advanced Quality):**
- JobBOSS² Advanced Quality by uniPoint is a purpose-built QMS that integrates natively with JobBOSS
- Features: vaulted document environment, strict revision control, digital signatures, date/time stamps on approvals, non-revocable approvals, encrypted passwords
- Non-Conformance Report (NCR) generation
- Employee training records
- Audit trail documentation for certification audits
- Real-time tracking of each production stage for issue identification

**User Feedback on Quality:**
- Aerospace and defense manufacturers specifically praise the template system for certification needs
- Users report being able to "make and store templates to their business needs and print numerous reports — we love the templates we were able to make and add to the program specifically for our certification needs"
- The uniPoint integration is described as making AS9100 certification achievable for small shops that previously couldn't afford the administrative overhead
- Some users note that without uniPoint, the native quality tools are basic

**Limitation:** The full quality suite (uniPoint) is an add-on with additional cost, which some small shops find financially burdensome.

---

### 6.11 Mobile Apps & DCMobile

**Overview:**
JobBOSS offers two primary mobile apps:
1. **JobBOSS² Data Collection App** (App Store ID 1213396029)
2. **JobBoss² Employee DC App** (App Store ID 1287589083)

**What Works:**
- Clock in/out of jobs via mobile device
- Material issuance and receipt recording
- Job status updates from the shop floor
- Barcode scanning for job and material transactions
- Available for iOS

**Documented Problems:**

| Problem | Severity | Status |
|---|---|---|
| App crashes when viewing job attachments on iPhone | High | Ongoing — reported to work ~30% of the time |
| Camera functionality broken after rebranding update | High | Reported unresolved |
| Login failures after software updates | High | Cases open 1+ month without resolution |
| No default action setting for scanner | Medium | Reported in Ideas Portal DCMOBILE-I-1 |
| Attachments cannot be viewed through DCMobile | Medium | Requested 2+ years ago in DCMOBILE-I-10 — not yet implemented |
| Recurring system glitches and slow response | Medium | Ongoing |

**User Experience Quotes:**
- "There seems to be glitches when you view documents uploaded to a job on the iPhone. Click to view and the app closes. However, it is a nuisance and becomes frustrating. This works 30% of the time."
- "A recent update broke logging in for some users who depend on the app daily for keeping track of time, with customer support slow to respond and cases remaining unresolved for over a month."

---

## 7. KNOWN TECHNICAL LIMITATIONS & FIELD CONSTRAINTS

This section documents hard technical constraints that users cannot work around without custom development or database modification.

### Field Length Limitations (Confirmed from Multiple Sources)

| Field | Hard Limit | Impact |
|---|---|---|
| Material ID / Part Number (displayed) | 30 characters | Cannot accommodate manufacturer + part number on PO |
| Material ID (typeable) | 50 characters | Extra characters are invisible on PO — data loss risk |
| Material Description | 30 characters | Identical truncated descriptions for variant parts |
| Drawing Number | 16 characters | Insufficient for aerospace/defense drawing numbers |
| Revision Field | 8 characters | Aerospace ADCN/revision numbers exceed this regularly |
| Job Number | Limited | Users requesting expansion (JBCORE-I-377, JBCORE-I-12) |

### Ideas Portal Requests for Field Expansion (Active as of 2025)

- **JBCORE-I-81**: Expand Character Limit — Material IDs
- **JBCORE-I-290**: Increase character limit — Material Description
- **JBCORE-I-377**: Expand the amount of characters for a job number
- **JBCORE-I-1283**: Increase the length of the Revision field for Jobs
- **JBCORE-I-12**: Larger field for "Job"
- **JBCORE-I-1234**: Have the ability to modify field sizes in JobBoss (general)

### Vendor Reference Field Conflict
- If a value is entered in the Vendor Reference field or Detail Number field on a purchase order, it **overwrites** the Extended Description space on the PO
- This is a known UI design conflict that forces users to choose between vendor reference tracking and extended item descriptions

### QuickBooks Sync Limitations
- Sales Tax information: NOT synced
- Payment Terms: NOT synced
- Vendor Cash Discounts: NOT supported (QuickBooks Online version)

### Reporting Constraints
- Crystal Reports UFL (User Function Library) is proprietary — formula errors require ECI support
- Stimulsoft embedded version: Stimulsoft itself cannot provide user support
- No built-in report editor (requires Crystal Reports license purchase or Stimulsoft-direct license)

### Support Hours
- Phone support: 7am–7pm CST Monday–Friday only
- No 24/7 support
- No weekend support

### Scalability
- Large BOM complexity (600–700 components) causes performance and usability issues
- High user counts (100+) make material transaction attribution difficult without the Material Transaction User Tracking Utility

---

## 8. PERFORMANCE & DATABASE ISSUES

### System-Wide Performance Complaints

User reports of system slowness appear in community forums and review sites:
- "The program is very slow, so much so that users click an icon again without realizing their first request is still being processed" — Capterra review
- Server-side crashes reported in legacy on-premise deployments
- Web app described as "finicky" with occasional unexplained user logouts

### Crystal Reports Performance — Technical Root Cause

The most thoroughly documented performance issue is Crystal Reports report generation speed. The root cause is well-understood in the Crystal Reports community:

**Problem:** A report built with multiple Command objects causes Crystal Reports to execute multiple queries and then join the results in memory on the workstation, rather than delegating the join to SQL Server.

**Symptoms:** Reports that should complete in seconds take 2–4+ minutes.

**Solution:** Rewrite the report with a single Command that contains a unified SQL query with all necessary joins and WHERE clause filtering. This pushes computation to SQL Server, which handles it dramatically faster.

**Practical guidance from the SAP Community:**
- Use WHERE clause in the Command to filter data — not the Select Expert in Crystal
- The Select Expert filters after data is pulled into memory; WHERE filters at the database
- If multiple Commands are absolutely necessary, explore whether a stored procedure or view can consolidate them

### SQL Server General Performance Considerations

While not JobBOSS-specific, the following SQL Server performance factors are relevant for on-premise JobBOSS installations:
- Missing or fragmented indexes force table scans instead of index seeks
- Insufficient buffer pool memory forces excessive disk I/O
- CPU bottlenecks from poorly optimized queries (especially in Crystal Reports)
- I/O bottlenecks from high-frequency reads and writes during peak production data collection periods

### Cloud Version Performance

The cloud (JobBOSS²) version removes on-premise database administration burden but introduces:
- Internet connectivity dependency — poor connectivity means poor performance
- Session timeout issues — users report being logged out unexpectedly
- Browser compatibility considerations

---

## 9. CUSTOMER SUPPORT & IMPLEMENTATION FEEDBACK

### Support Channel Overview

| Channel | Availability | Notes |
|---|---|---|
| Phone Support | 7am–7pm CST, M–F only | No weekend or overnight coverage |
| Online Knowledge Base | 24/7 | Self-service articles and videos |
| Customer Portal | 24/7 | Ticket submission, utilities, resources |
| Training (ECI) | Scheduled sessions | Online and onsite options |
| User Community | Business hours focus | Ideas portal, community forum |
| Third-Party Partners | Varies | Chortek, ReportingGuru, others |

### Support Quality — Bimodal Distribution

The data strongly suggests that support quality is bimodal — users either have excellent experiences or very poor ones, with limited middle ground:

**Positive Experiences (Documented):**
- "Fast, knowledgeable, and friendly support staff who resolve issues efficiently"
- "Support always jumps in to assist if needed"
- Implementation people are generally described as good
- Training resources (videos, online) described as helpful

**Negative Experiences (Documented):**
- Initial support response on a major inventory error: 3+ weeks with repeated follow-ups
- Custom report request: weeks for initial feedback
- Implementation described as "atrocious" for customer service (though tech support was praised)
- Customer service rep described as "late or non-responsive"
- Post-acquisition (ECI), perceived decline in support quality
- Mobile app login issues: cases open 1+ month without resolution
- User described 45 years of software implementations — said this was the worst

### Implementation Patterns

**What Successful Implementations Have in Common:**
- Dedicated internal champion who owns the implementation
- Users dedicate team members to training who then become internal trainers
- Realistic timeline expectations
- Pre-implementation data cleanup (customer, vendor, part master data)
- SOPs written for all key processes before go-live

**What Failed Implementations Have in Common:**
- Relying entirely on ECI for all training and setup
- Minimal internal ownership
- Going live without testing edge cases (blanket orders, complex scheduling, QuickBooks sync)
- Skipping data migration planning

### Third-Party Resources Available

- **Chortek Consulting**: Publishes detailed multi-part integration guides for JobBOSS²/QuickBooks Online
- **ReportingGuru.com**: Custom Crystal Reports development specifically for JobBOSS
- **TITANS of CNC Academy**: JobBOSS scheduling training content
- **Mingo Smart Factory**: Performance calculation guides specific to JobBOSS

---

## 10. PRICING & COST CONCERNS

### Pricing Structure (as of 2025–2026)

ECI does not publicly disclose JobBOSS² pricing. Based on third-party sources:

| Cost Element | Reported Range |
|---|---|
| Per-user monthly subscription | Starting ~$200/user/month |
| Implementation services | Starting ~$5,000 (varies with complexity) |
| Custom report development | Additional cost; quoted per project |
| Advanced Quality (uniPoint) | Add-on cost; not included in base |
| Cloud migration from on-premise | Significant cost; multiple cases of $10K+ |

### Historical Price Trajectory (Forum Data)

- **2012 era**: ~$1,200/seat (reported by Practical Machinist forum users)
- **Post-ECI acquisition**: ~$3,600/seat reported by same long-term users
- **One documented case**: Total cost escalated from ~$10,000 to "well over $40,000" post-acquisition while key modules remained non-functional

### Cost Complaints Breakdown

**Rising Maintenance Fees:**
Users on annual maintenance contracts report fees increasing significantly year over year since the ECI acquisition.

**Feature Gating:**
Certain features require extra payments beyond the base subscription — users find this confusing and burdensome for smaller companies.

**Support Costs:**
Extended or priority support requires additional payment.

**Custom Report Costs:**
Custom report development must be purchased through ECI or third parties — there is no self-service path that doesn't require Crystal Reports expertise.

**Subscription Lock-In:**
One documented case: when a customer voiced intent to switch providers, they were told to organize everything in one week, after which the subscription was shut off until they paid for a year contract.

**Migration Costs:**
Moving from on-premise to cloud requires converting G/L history, customer records, vendor records, and invoices — described as complex and expensive.

---

## 11. USER WORKAROUNDS & TIPS

This section aggregates practical workarounds shared by users across all review platforms and community forums.

### Workaround: Blanket Orders with Multiple Delivery Dates
**Problem:** The scheduler tries to schedule the entire blanket order quantity at once.
**Solution:** Enter the master order, then use the Job Split function to create sub-jobs (R1, R2, etc.) for each release date and quantity. Alternatively, use the Sales Order module which handles multi-line delivery dates more gracefully.

### Workaround: Crystal Reports Performance
**Problem:** Custom Crystal Report takes 2–4+ minutes to generate.
**Solution:** Consolidate all data pulls into a single SQL Command within Crystal Reports. Move all filtering logic into the WHERE clause of that command rather than using Crystal's Select Expert. This pushes computation to SQL Server rather than in-memory processing.

### Workaround: Crystal Reports Script Error on Import
**Problem:** User-defined Crystal Report throws a script error when run in JobBOSS.
**Solution:** Ensure the UD_Report.htm file is identical in both the "Reporting" and "Reporting\User-Defined" directories. Verify that the Report Name exactly matches the Title field in Crystal Reports Document Properties.

### Workaround: QuickBooks Sales Tax Missing
**Problem:** Sales Tax information is not passed from JobBOSS to QuickBooks.
**Solution:** Establish a manual process to set tax rates on all new customer records in QuickBooks immediately upon creation. Consider this a permanent operating procedure, not a temporary fix.

### Workaround: Material Inventory Accuracy
**Problem:** Inventory counts drift because shop floor workers skip receiving and issuing steps.
**Solution:** Use barcode labels on all material bins and job travelers. Implement the Material Transaction User Tracking Utility to identify users who skip steps. Conduct regular cycle counts using the Physical Inventory module. Write formal SOPs and enforce compliance.

### Workaround: Part Number Character Limit
**Problem:** 30-character display limit on Material IDs truncates manufacturer part numbers.
**Solution:** Develop a standardized internal part numbering system that is distinct from manufacturer part numbers. Store manufacturer part numbers in the Vendor Reference or extended description fields. Be aware of the Vendor Reference / Extended Description field conflict on POs.

### Workaround: Scheduling Multi-Machine Jobs
**Problem:** Multi-pallet or multi-machine jobs confuse the scheduler.
**Solution:** Create separate work center routing steps for each machine/pallet operation. Accept that the built-in scheduler will not produce a perfect multi-machine schedule and supplement with a manual whiteboard or external scheduling tool for these specific job types.

### Workaround: DCMobile Attachment Viewing
**Problem:** App crashes when viewing documents attached to a job.
**Solution:** Access job attachments through the desktop web browser for reliable viewing. Use DCMobile only for time entry, material transactions, and job status — not document review.

### Workaround: Packing List Complexity
**Problem:** Creating a packing list takes 4 steps; correcting errors requires 8 more steps.
**Solution:** Verify all shipment information carefully before initiating the packing list workflow. Create an internal checklist for shipping clerks to verify job status, quantities, and addresses before beginning the packing list process.

### Workaround: Identifying Inventory Transaction Users
**Problem:** With 100+ users, it's hard to know who moved which material.
**Solution:** Use the Material Transaction User Tracking Utility (available on the Customer Portal) to run periodic audits. Require all material transfers via barcode scanner with authenticated login, as scanner transfers provide better audit trails than PC entry.

### Workaround: Estimating Accuracy
**Problem:** Estimates drift from actuals over time.
**Solution:** Run the Estimated vs. Actual report regularly by job type, customer, and work center. Use the findings to update estimating templates quarterly.

### Workaround: QuickBooks Integration Complexity
**Problem:** Integration is described as requiring 15+ steps.
**Solution:** Consider using JobBOSS for operations only and QuickBooks Online for all financial management. Batch sync financial transactions on a scheduled basis (daily or weekly) rather than attempting real-time sync.

---

## 12. JOBBOSS IDEAS PORTAL: TOP FEATURE REQUESTS

The Ideas Portal (ideas.jobboss.com) is a publicly accessible platform where customers submit and vote on feature requests. The following have been identified as active and notable requests:

### Core System Requests

| Idea ID | Request | Notes |
|---|---|---|
| JBCORE-I-1418 | Built-In Report Editor | Avoid need for Crystal Reports license; create/edit report versions within software |
| JBCORE-I-303 | JobBoss History | Full audit trail of all changes, deletions, and transactions with user attribution |
| JBCORE-I-483 | Inventory Transfer Traceability | Merged into JBCORE-I-303 |
| JBCORE-I-1234 | Ability to Modify Field Sizes | General ability to resize input fields |
| JBCORE-I-81 | Expand Material ID Character Limit | Increase from 30 characters |
| JBCORE-I-290 | Increase Material Description Character Limit | Increase from 30 characters |
| JBCORE-I-377 | Expand Job Number Character Limit | Current limit causes issues |
| JBCORE-I-1283 | Increase Revision Field Length | 8 characters insufficient for aerospace |
| JBCORE-I-12 | Larger Job Field | General expansion |
| JBCORE-I-1264 | Make Job Part Number and Description Always Visible | Persistent display across screens |
| JBCORE-I-299 | Add Pictures to Individual Job Routing | Visual work instructions on travelers |
| JBCORE-I-438 | QuickBooks Integration to Pass Tax Info and Payment Terms | Critical missing sync feature |

### DCMobile Requests

| Idea ID | Request | Notes |
|---|---|---|
| DCMOBILE-I-1 | Better Scanner Software (Set Default Action) | No ability to set scanner post-scan default |
| DCMOBILE-I-10 | Allow Attachments to Be Viewed Through DC Mobile | Requested 2+ years ago; not yet implemented |

### Key Pattern in Feature Requests

The Ideas Portal reveals a clear pattern: users want **more transparency** (audit trails, history), **less restriction** (field sizes, data entry limits), **better reporting** (built-in editor, visual dashboards), and **better mobile** (attachments, scanner behavior). These directly mirror the top complaints identified in formal reviews.

---

## 13. COMPETITIVE COMPARISONS

### 13.1 JobBOSS vs E2 Shop System

**Historical context:** These are now the same product. ECI acquired both and merged them into JobBOSS². However, users of the legacy E2 on-premise system and users of legacy JobBOSS on-premise system may still encounter both products in comparison discussions.

| Dimension | JobBOSS (Legacy) | E2 Shop System (Legacy) |
|---|---|---|
| UI Design | Older, more traditional | More graphical, considered easier for self-training |
| Cloud Native | No (older versions) | Yes (E2 was cloud-native) |
| Pricing (historical) | Per-seat license | $4,995 per license (historical) |
| User Sentiment | Generally positive | Generally positive |
| Reason ECI Acquired E2 | — | "Intuitive UI and cloud-native capabilities" |

**What JobBOSS² Represents:** The merger takes the deep job shop workflow of JobBOSS and adds E2's modern UI and cloud architecture. Users of both legacy products generally see the combined product as an improvement.

**User Quote (Practical Machinist):** "JobBoss is more graphical with quicker self-training compared to E2."

---

### 13.2 JobBOSS vs ProShop ERP

This is the most actively discussed comparison in current manufacturing forums and is increasingly the primary competitive context for JobBOSS.

| Dimension | JobBOSS² | ProShop ERP |
|---|---|---|
| G2 Ease of Use | 7.4 | 8.8 |
| G2 Quality of Support | 7.4 | 9.0 |
| User Satisfaction (SelectHub) | 83% (1,012 reviews) | 99% (65 reviews) |
| G2 Work Order Management | — | 9.6 |
| Target Market | Small-mid job shops | Small-mid job shops |
| QMS Integration | Via uniPoint (add-on) | Native |
| Interface Modernity | Criticized as dated | Praised as modern |

**Key insight from Practical Machinist forum thread "ERP - JobBoss^2 vs. ProShop vs. Something Actually Good?":**
- Users switching from E2/JobBOSS to ProShop reported significant improvements
- One user specifically noted 40% revenue growth in the past year after switching to ProShop, averaging 95% on-time delivery, shipping 170 orders/month with 51 employees and 22 machines
- However, ProShop has a much smaller review base (65 vs. 1,012 on SelectHub) — the 99% satisfaction rating may reflect a self-selection bias of highly committed users

**ProShop's own positioning** (from proshoperp.com/replacing-jobboss/): ProShop explicitly markets itself as a JobBOSS replacement, indicating the competitive intensity of this comparison.

**The Price Factor:** ProShop is generally higher-priced than JobBOSS², making it less accessible for very small shops. JobBOSS² maintains a price advantage at the entry level.

---

### 13.3 JobBOSS vs Other Alternatives

**vs. Shoptech M1 (ECI's Other Product):**
Slashdot lists M1 as a primary comparison. M1 is ECI's mid-market manufacturing ERP, positioned above JobBOSS² in complexity and price.

**vs. NetSuite:**
NetSuite is significantly more expensive and complex — generally not a direct competitor at JobBOSS²'s target market. However, users who outgrow JobBOSS² sometimes cite NetSuite as the next step. G2 comparison pages exist for this pairing.

**vs. MRPEasy:**
MRPEasy is a simpler, lower-cost alternative targeted at very small manufacturers. SelectHub has published a comparison, with MRPEasy generally winning on simplicity and price, JobBOSS² on depth of job shop functionality.

**vs. Infor LN:**
Another mid-to-large enterprise comparison that shows up in SelectHub comparisons. Not a direct competitor for most JobBOSS² users.

**vs. SAP ERP:**
SAP is enterprise-grade; the comparison is primarily relevant for larger manufacturers evaluating whether to go with an enterprise system or a specialized job shop system.

---

## 14. USER QUOTES FROM REAL REVIEWS

The following quotes are sourced from identified review platforms. They represent the range of user experience from strongly positive to strongly negative.

---

**On Overall Workflow:**
> "The best feature for many users is how it flows through the system — from estimating to actually creating an order to shipping to receiving to billing. The workflow is very easy to understand."
— Capterra reviewer

---

**On Job Tracking:**
> "JobBOSS² allows users to follow job status in real-time from the work center update, making it easy to tell which workstation is behind or ahead of schedule."
— Capterra reviewer

---

**On Ease of Use:**
> "Ease of use is the best around and the workflow is logical for a JobShop environment. There is not a single person in the shop that doesn't know or interact with it on a daily basis."
— G2 reviewer

---

**On Scalability:**
> "The system can handle micro businesses with ease, but also grows with you. Every time there's a need for something more [like moving to the cloud] JobBoss2 is right there with the next evolution of their product."
— G2 reviewer

---

**On Estimating:**
> "Users have been able to utilize the estimating (bill of materials / labor) system to effectively quote customers."
— Synthesized from multiple reviews

---

**On QuickBooks Integration:**
> "The Accounting can be a little wonky sometimes in how it integrates with QuickBooks."
— G2 reviewer

---

**On Reporting:**
> "Users frequently express dissatisfaction with the reporting features of JobBOSS², citing limitations in customization and data extraction."
— SelectHub aggregate

---

**On Support (Negative):**
> "Poor experience with customer support and communication to resolve software issues. A recent example is with a system-caused large inventory error. Initial response with repeat follow-up took over 3 weeks, and I was reaching out to others after 4 weeks."
— Capterra reviewer

---

**On Support (Positive):**
> "Fast, knowledgeable, and friendly support staff who resolve issues efficiently."
— G2 reviewer

---

**On Implementation (Severe Negative):**
> "It has been the worst implementation in my 45 years of implementing manufacturing software systems. Our customer service rep is late or non-responsive to our inquiries about the software. Software is very limited on flexibility adapting to a company's industry needs."
— Capterra reviewer (1-star)

---

**On Pricing Post-ECI:**
> "ECI is only about money — those with issues or needs should expect to pay big $$."
— Synthesized from user forum comments

> "Costs went from $10K to well over $40K, and purchasing and scheduling still didn't work."
— User on pricing escalation

---

**On Scheduling Limitations:**
> "If you run multiple jobs on a single machine or use multi-pallet machines, scheduling can get frustrating, and you will have to come up with your own solutions to schedule those machines."
— Capterra reviewer

---

**On Field Lengths:**
> "The 30-character material limit is often too short to fit the manufacturer and their part number together without losing important part data."
— Ideas Portal (JBCORE-I-81)

> "Users end up with a significant number of items with the exact same description since they get cut off after 30 characters — fabrication and integration shops are driven crazy by it."
— Ideas Portal (JBCORE-I-290)

---

**On Packing Lists:**
> "Making a packing list requires 4 steps, and once made the job is closed, requiring 4 steps to undo and 4 steps to redo — this should be simpler."
— Ideas Portal / Capterra reviewer

---

**On DCMobile:**
> "There seems to be glitches when you view documents uploaded to a job on the iPhone. Click to view and the app closes. However, it is a nuisance and becomes frustrating. This works 30% of the time."
— App Store / Capterra reviewer

---

**On Shop Floor Adoption:**
> "Shop floor workers may develop defense mechanisms when they realize they're being tracked, such as scanning out early to appear as fewer hours."
— Practical Machinist forum user

---

**On Switching to ProShop:**
> "We switched from E2 and achieved 40% revenue growth in the past year while averaging 95% on-time delivery shipping 170 orders a month as a high-mix low-volume shop with 51 employees and 22 machines."
— Practical Machinist forum user post-migration

---

**On the Copy/Paste Problem:**
> "Enable standard copy/paste functionality (CTRL+C and CTRL+V) for materials and routings — the current copy/paste utility is clunky at best."
— Ideas Portal user

---

**On Templates and Certifications:**
> "We love the templates we were able to make and add to the program specifically for our certification needs."
— TrustRadius reviewer (AS9100 user)

---

**On the Blanket Order Workaround:**
> "The intent of blanket orders is to allow the schedule to schedule to the release size rather than trying to schedule the entire order. Use the Job Split function to enter release dates and quantities."
— Practical Machinist forum user (experienced user tip)

---

## 15. WHO IS THIS SOFTWARE BEST / WORST FOR

### Best Fit: JobBOSS / JobBOSS²

**Industry Type:**
- Custom / make-to-order job shops
- Contract machine shops
- Metal fabricators
- Precision parts manufacturers
- Shops pursuing or maintaining ISO 9001 / AS9100 certification (with uniPoint add-on)

**Company Size:**
- 1–50 employees: Strong fit
- 51–150 employees: Adequate fit with some growing pains
- 150+ employees: Potential scalability concerns; evaluate carefully

**Operational Profile:**
- High-mix, low-volume production
- Quote-to-order workflow is central to operations
- Job costing by order is essential
- Multiple departments need integrated visibility (sales, shop floor, purchasing, accounting)
- Using QuickBooks for accounting (note: integration has documented issues; see Section 6.8)

**Technology Sophistication:**
- Small shops without dedicated IT staff benefit from the cloud version
- Companies comfortable with Crystal Reports or willing to hire third-party report developers
- Companies willing to invest in proper implementation and training

---

### Poor Fit: JobBOSS / JobBOSS²

**Don't Choose JobBOSS² If:**
- You have complex multi-level assemblies with 600+ components
- You run high volumes of multi-pallet or multi-machine jobs requiring precise scheduling
- You need 24/7 support or cannot tolerate multi-week response times
- Your part numbers, drawing numbers, or revision codes exceed the character limits
- You need advanced analytics and reporting without custom development investment
- Your QuickBooks integration requires sales tax and payment terms sync to work automatically
- You are a large enterprise (150+ employees) with complex multi-site operations
- You have complex sales tax scenarios across multiple jurisdictions
- You require a modern, visually sophisticated UI comparable to consumer-grade software

---

## 16. SUMMARY VERDICT

### Strengths That Are Real and Consistent

1. **End-to-end job shop workflow**: Quote-to-invoice process is genuinely well-designed for make-to-order shops
2. **Job costing accuracy**: Real-time labor and material cost tracking by job
3. **Ease of onboarding** (JobBOSS² version): New employees can learn the system quickly
4. **Module breadth**: One system can run all departments of a small shop
5. **Estimating module**: Template-based quoting with AI BOM builder is strong
6. **Scheduling visibility**: When used correctly for standard job types, provides meaningful shop floor visibility
7. **Quality support** (uniPoint): Strong path to ISO/AS9100 compliance for small shops

### Weaknesses That Are Real and Persistent

1. **Customer support quality**: Inconsistent and slow, particularly for complex issues post-ECI acquisition
2. **Reporting**: Significant gap between user needs and out-of-box capabilities; requires Crystal Reports expertise or ECI consulting spend
3. **Field length restrictions**: 30-character part numbers and 16-character drawing numbers create ongoing operational friction for many shops
4. **QuickBooks integration**: Missing sales tax and payment terms sync is a fundamental gap
5. **Scheduling complexity**: Multi-machine and blanket order scenarios require workarounds
6. **Mobile app reliability**: DCMobile has persistent bugs with no timely resolution
7. **Pricing trajectory**: Post-ECI acquisition pricing increases have damaged loyalty and value perception
8. **Character-limit design debt**: Multiple ideas portal requests for field expansion suggest a backlog of long-standing technical debt

### Overall Assessment

JobBOSS² occupies a well-earned position in the small-to-mid job shop market. For a shop that fits its sweet spot — 5 to 50 employees, make-to-order manufacturing, QuickBooks as the accounting system — it provides strong value and genuine operational improvement. The 83% satisfaction rate on SelectHub (1,012 reviews) and the 8.0/10 on TrustRadius for the current JobBOSS² version reflect a genuinely useful product.

However, the satisfaction ceiling appears to be real. Users who outgrow the standard assumptions — who need complex scheduling, advanced reporting, long part numbers, or weekend support — consistently hit limitations that no amount of user skill can fully overcome without custom development investment. The ECI acquisition has introduced pricing pressure that is straining the value proposition, and support responsiveness appears to have declined.

For shops seriously evaluating alternatives, ProShop ERP represents the most directly comparable modern competitor with notably higher satisfaction scores — though at higher price and lower volume of user reviews. For shops making their first ERP selection, JobBOSS² remains a credible choice with a mature feature set and a large installed base providing substantial community knowledge.

---

## SOURCES

- [JobBOSS² Reviews 2026 — Capterra](https://www.capterra.com/p/219273/JobBOSS/reviews/)
- [JobBOSS² Reviews 2026 — G2](https://www.g2.com/products/jobboss2/reviews)
- [JobBOSS² Reviews — GetApp](https://www.getapp.com/industries-software/a/jobboss/reviews/)
- [Pros and Cons of JobBOSS 2025 — TrustRadius](https://www.trustradius.com/products/jobboss/reviews?qs=pros-and-cons)
- [Pros and Cons of JobBOSS² 2025 — TrustRadius](https://www.trustradius.com/products/jobboss2/reviews?qs=pros-and-cons)
- [JobBOSS² Reviews — SoftwareAdvice](https://www.softwareadvice.com/manufacturing/jobboss2-profile/reviews/)
- [JobBOSS² Reviews 2025 — SelectHub](https://www.selecthub.com/p/manufacturing-software/jobboss/)
- [JobBOSS Reviews, Pricing & Features — TEC](https://www3.technologyevaluation.com/solutions/48771/jobboss)
- [JobBOSS² Shop Management Review — SoftwareConnect](https://softwareconnect.com/reviews/jobboss-erp/)
- [JobBOSS Review 2025 — Nerdisa](https://nerdisa.com/jobboss/)
- [JobBOSS² ERP Review — Top10ERP](https://www.top10erp.org/products/jobboss%C2%B2)
- [ERP: JobBoss^2 vs. ProShop vs. Something Actually Good? — Practical Machinist](https://www.practicalmachinist.com/forum/threads/erp-jobboss-2-vs-proshop-vs-something-actually-good.411796/)
- [ERP Showdown: Jobboss or E2 — Practical Machinist](https://www.practicalmachinist.com/forum/threads/erp-show-down-jobboss-or-e2-current-feelings-on-both.366698/)
- [Using Job Boss — Practical Machinist](https://www.practicalmachinist.com/forum/threads/using-job-boss.320829/)
- [JobBoss Scheduling/Blanket Orders — Practical Machinist](https://www.practicalmachinist.com/forum/threads/jobboss-scheduling-blanket-orders.261436/)
- [Crystal Reports Custom Report Taking 4 Minutes from JobBoss — SAP Community](https://community.sap.com/t5/technology-q-a/crystal-reports-custom-report-taking-four-minutes-to-generate-from-jobboss/qaq-p/12609257)
- [Formula Error When Modifying JobBoss Report in Crystal Reports — SAP Community](https://community.sap.com/t5/technology-q-a/formula-error-when-modifying-jobboss-report-in-crystal-reports/qaq-p/12610292)
- [JobBoss Throws a Script Error on User-Defined Report — SAP Community](https://community.sap.com/t5/technology-q-a/jobboss-throws-a-script-error-when-i-try-to-run-my-user-defined-report/qaq-p/12580336)
- [Stimulsoft Forum — Report Designer in JobBOSS2](https://forum.stimulsoft.com/viewtopic.php?t=61268)
- [Custom JobBOSS Reports & Dashboards — ReportingGuru](https://reportingguru.com/jobboss-custom-reports/)
- [JobBOSS2 and QuickBooks Online Integration — Chortek](https://www.chortek.com/blog/jobboss2-quickbooks-online-integration-benefits/)
- [JobBOSS Ideas Portal](https://ideas.jobboss.com/)
- [Inventory Transfer Traceability — Ideas Portal JBCORE-I-483](https://ideas.jobboss.com/ideas/JBCORE-I-483)
- [Built-In Report Editor — Ideas Portal JBCORE-I-1418](https://ideas.jobboss.com/ideas/JBCORE-I-1418)
- [JobBOSS Bar Code Scanner — Practical Machinist](https://www.practicalmachinist.com/forum/threads/jobboss-bar-code-scanner.389347/)
- [Replacing JobBoss with ProShop ERP](https://proshoperp.com/replacing-jobboss/)
- [E2 Shop System vs. JobBOSS Comparison — SourceForge](https://sourceforge.net/software/compare/E2-Shop-System-vs-JobBOSS/)
- [JobBOSS² vs ProShop ERP — SelectHub](https://www.selecthub.com/manufacturing-software/jobboss-vs-e2-shop-system/)
- [JobBOSS² vs E2 Shop System — SelectHub](https://www.selecthub.com/manufacturing-software/jobboss-vs-e2-shop-system/)
- [JobBoss² Employee DC App — App Store](https://apps.apple.com/us/app/jobboss-employee-dc/id1287589083)
- [JobBOSS Streamline ISO 9001 & AS9100 with uniPoint — ECI](https://www.ecisolutions.com/blog/manufacturing/jobboss/how-jobboss-and-unipoint-support-iso-9001-and-as9100-certification/)
- [JobBOSS Material and Inventory Control — ECI](https://jobboss.com/job-shop-manufacturing-software/jobboss/material-and-inventory-control-software-for-manufacturing)
- [Material Transaction User Tracking Utility — JobBOSS Customer Portal](https://customers.jobboss.com/material-transaction-user-tracking-utility)


---

## Document Generation Notes

This reference manual was compiled from:
- Official ECI Solutions / JobBOSS documentation
- ECI customer portal knowledge base articles
- YouTube training videos (TITANS of CNC Academy, ECI Solutions channel)
- "Mastering JobBoss" (Innoware PJP, 2024, ISBN 9798322488682)
- User reviews: Capterra, G2, TrustRadius, GetApp, SelectHub
- Community forums: Reddit r/manufacturing, r/smallbusiness
- Technical schema analysis
- ReportingGuru.com resources
- Integration vendor documentation (Makini, Paperless Parts, Alora, Costimator)

**For Karen:** When in doubt about any procedure, call ECI support first. The phone number and portal are on your welcome email from ECI. This document is a reference — ECI support is the authoritative source for version-specific behavior.

**Generated:** 2026-02-25 | Die-Mension LLC | Brunswick, OH
