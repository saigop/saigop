# Admin & Marketing Manager Dashboard Specification

## Document Overview
This specification outlines the functional requirements for two primary user personas:
1. **Admin Persona** - System administration, configuration, and monitoring (240 hours)
2. **Marketing Manager Persona** - Analysis, reporting, and competitive insights (280 hours)

**Total Effort**: 520 hours

---

## Admin Persona Specification

**Total Hours**: 240 hours

### Overview
The Admin Persona focuses on system administration, configuration, data source management, and monitoring. This role ensures system health, manages entities and competitors, configures reporting, and oversees user management with audit capabilities.

### 1. Login and Dashboard Overview (40 hours)

#### Description
Develop secure login using AWS Cognito with MFA support. Create dashboard displaying system health including user count, data sources, schedules, last sync status, and error logs via CloudWatch. Incorporate role-based permissions and quick action buttons for management tasks.

#### Functional Requirements
- **Authentication**
  - AWS Cognito integration for user authentication
  - MFA support (SMS, TOTP, or email-based)
  - Secure session management with token refresh
  - Password reset and recovery flows

- **Dashboard Metrics Display**
  - Total active users count
  - Number of configured data sources
  - Active schedules count
  - Last successful sync timestamp
  - Error logs integration via AWS CloudWatch
  - System uptime and health indicators

- **Role-Based Access Control (RBAC)**
  - Permission levels: Super Admin, Admin, Viewer
  - Role-based feature visibility
  - Audit trail for permission changes

- **Quick Actions Panel**
  - Trigger manual sync
  - View recent errors
  - Access frequently used configurations
  - System health refresh

#### Acceptance Criteria
- [ ] User can login with username/password
- [ ] MFA challenge prompts on login (if enabled)
- [ ] Dashboard displays all key metrics within 2 seconds
- [ ] CloudWatch logs are accessible and filterable
- [ ] Role-based permissions restrict unauthorized access
- [ ] Quick actions execute successfully with confirmation

---

### 2. Data Sources and Schedules Management (30 hours)

#### Description
Create UI for viewing and configuring scheduled jobs for Glue ETL operations. Support fixed schedules only (no dynamic scheduling). Add parameters for API keys and maximum competitor limits. Enable manual sync triggers and performance metrics monitoring. Integrate with EventBridge for job scheduling. Reduced scope: Start with fixed scheduling only.

#### Functional Requirements
- **Schedule Viewing**
  - List all configured schedules (name, frequency, last run, status)
  - Visual calendar view of upcoming jobs
  - Status indicators: Active, Paused, Failed, Completed

- **Configuration Management**
  - Schedule creation wizard
  - Configure parameters:
    - API keys (encrypted storage)
    - Maximum competitor count per job
    - Data source type (Google Reviews initially)
    - Frequency (fixed schedules only in v1)
  - Edit existing schedules
  - Enable/disable schedules

- **Manual Sync Triggers**
  - On-demand execution button
  - Confirmation dialog with estimated runtime
  - Real-time progress tracking
  - Notification on completion/failure

- **Performance Metrics**
  - Records processed count
  - Execution duration
  - Success/failure rates
  - Resource utilization graphs

- **AWS Integration**
  - EventBridge for schedule orchestration
  - Glue ETL job monitoring
  - CloudWatch metrics integration

#### Acceptance Criteria
- [ ] Display list of all configured schedules
- [ ] Create new schedule with all required parameters
- [ ] Edit existing schedule configurations
- [ ] Manual sync triggers successfully
- [ ] Performance metrics update in real-time
- [ ] Fixed schedules execute as configured via EventBridge

---

### 3. Entities and Competitors Management (60 hours)

#### Description
Build management interface for retailers and shopping centers. Support toggle for 'Treat as Retailer' functionality for shopping centers and set display order. Implement dynamic competitor list management supporting up to 20 competitors with search and add functionality via Google API. Enable on-demand data ingestion queuing via SQS for newly added competitors. Update data model in DynamoDB and OpenSearch.

#### Functional Requirements
- **Entity Management**
  - List view of all entities (retailers and shopping centers)
  - Entity creation form:
    - Name, location, contact details
    - "Treat as Retailer" toggle for shopping centers
    - Display order configuration
    - Status (Active/Inactive)
  - Edit and delete capabilities
  - Bulk actions support

- **Competitor Management**
  - Dynamic competitor list per entity (max 20)
  - Search functionality via Google Places API
  - Add competitor form:
    - Business name
    - Location/address
    - Google Place ID
    - Auto-populate details from Google API
  - Remove competitor with confirmation
  - Reorder competitors by priority

- **Data Ingestion**
  - On-demand ingestion queue via SQS
  - Queue new competitor for data collection
  - Track ingestion status (Queued, Processing, Completed, Failed)
  - Retry failed ingestions

- **Data Model Updates**
  - DynamoDB schema for entities and competitors
  - OpenSearch index configuration
  - Relationship mapping (entity -> competitors)

#### Acceptance Criteria
- [ ] Create and manage retailers/shopping centers
- [ ] Toggle "Treat as Retailer" for shopping centers
- [ ] Set display order for entities
- [ ] Search and add competitors via Google API
- [ ] Manage up to 20 competitors per entity
- [ ] Queue new competitors for ingestion via SQS
- [ ] Track ingestion status for each competitor
- [ ] Data persists correctly in DynamoDB and OpenSearch

---

### 4. Reporting and Email Configuration (30 hours)

#### Description
Develop settings UI for scheduled PDF reports delivered via SES. Support weekly or monthly scheduling with configurable recipient lists per retailer (unrestricted number of recipients). Customize PDF report templates to show categories as rows and competitors as columns. Include test delivery functionality and sender management. Integrate with Lambda for PDF generation and EventBridge for automated scheduling. Simplified reporting scope for MVP.

#### Functional Requirements
- **Report Scheduling**
  - Configure weekly/monthly report schedules per retailer
  - Select report recipients per retailer (unrestricted email list)
  - Set delivery time and timezone
  - Enable/disable scheduled reports via checkbox per retailer
  - Email distribution lists stored per shop for automated delivery

- **Template Customization (Simplified v1)**
  - Basic PDF template structure
  - Company logo upload
  - Color scheme selection
  - Header/footer customization
  - Included metrics selection

- **Email Settings**
  - Sender email configuration (verified via SES)
  - Recipient list management
  - Email subject and body templates
  - Test email delivery functionality

- **Report Generation**
  - Lambda function for PDF generation
  - EventBridge for scheduling automation
  - S3 storage for generated reports
  - Delivery tracking and logs

- **Simplified Reporting Focus**
  - Streamlined metrics (core KPIs only)
  - Reduced complexity in templates
  - Essential visualizations only

#### Acceptance Criteria
- [ ] Schedule weekly/monthly reports
- [ ] Configure unrestricted recipient list
- [ ] Customize basic PDF template elements
- [ ] Send test email successfully
- [ ] Scheduled reports generate and deliver automatically
- [ ] Lambda function creates PDF reports
- [ ] Delivery logs accessible for audit

---

### 5. User Management and Monitoring (10 hours)

#### Description
Create basic user management section for adding, removing, and assigning roles. Integrate audit logs for key actions such as competitor additions and report generation. Add logout functionality and configuration export features. Use AWS Cognito user pool for authentication. Simplified scope: Basic Cognito integration without advanced features.

#### Functional Requirements
- **User Management (Basic Cognito)**
  - List all users with status
  - Add new users:
    - Email (username)
    - Temporary password generation
    - Role assignment
  - Remove users with confirmation
  - Assign/modify user roles
  - Enable/disable user accounts

- **Audit Logging**
  - Log critical events:
    - User login/logout
    - Competitor additions/removals
    - Report generation triggers
    - Configuration changes
  - Searchable audit log interface
  - Export logs to CSV
  - Retention policy configuration

- **System Features**
  - Logout functionality
  - Configuration export (JSON format)
  - Configuration import/restore
  - Session timeout settings

#### Acceptance Criteria
- [ ] Add new users to Cognito User Pool
- [ ] Assign roles to users
- [ ] Remove users from system
- [ ] Audit logs capture all critical events
- [ ] Search and filter audit logs
- [ ] Export configuration as JSON
- [ ] Logout terminates session correctly

---

### 6. Wireframes and Feedback Integration (20 hours)

#### Description
Update admin dashboard wireframes based on meeting feedback including system health views, error log displays, and considerations for future retailer access. Iterate on UI and UX designs to ensure transparency and ease of use.

#### Functional Requirements
- **Wireframe Refinement**
  - System health dashboard layout
  - Error log presentation format
  - Navigation structure optimization
  - Responsive design considerations

- **Feedback Implementation**
  - Stakeholder review sessions
  - Iteration on UI/UX based on input
  - Transparency in system status displays
  - Future-proofing for retailer access roles

- **Design Deliverables**
  - High-fidelity wireframes
  - Component library documentation
  - Interaction flow diagrams
  - Accessibility compliance notes

#### Acceptance Criteria
- [ ] Updated wireframes reflect meeting feedback
- [ ] System health views clearly display status
- [ ] Error logs are easily navigable
- [ ] Design accommodates future retailer role
- [ ] Stakeholder approval obtained

---

### 7. AI Rating Methodology Exposure (5 hours)

#### Description
Expose AI rating scale (Poor, Average, Good, Excellent) in admin views. Clarify rating methodology documentation. Note: Detailed LLM logic will be covered in Marketing Manager Persona.

#### Functional Requirements
- **Rating Scale Display**
  - Expose 4-level scale: Poor → Average → Good → Excellent
  - Admin view of rating thresholds
  - Methodology documentation link

- **Admin Controls**
  - View rating distribution across all entities
  - Access to rating calculation parameters
  - System-level rating statistics

- **Documentation**
  - Clear explanation of rating criteria
  - Dev team input integration
  - Methodology transparency for auditing

#### Acceptance Criteria
- [ ] AI rating scale visible in admin interface
- [ ] Rating methodology documented and accessible
- [ ] Admin can view rating distributions
- [ ] Methodology approved by dev team

---

### 8. Scheduled Data Collection Optimization (30 hours)

#### Description
Implement cron jobs for automated data collection with performance optimizations including batching. Monitor collection jobs and expose metrics in dashboard for transparency.

#### Functional Requirements
- **Cron Job Implementation**
  - EventBridge rules for scheduling
  - Lambda/Glue ETL job triggers
  - Configurable execution frequency

- **Performance Optimizations**
  - Batch processing for API calls
  - Request rate limiting
  - Concurrent execution controls
  - Error handling and retries

- **Monitoring**
  - Execution duration tracking
  - Success/failure rates
  - Resource utilization metrics
  - Cost tracking per job

- **Dashboard Integration**
  - Real-time job status
  - Performance graphs (execution time trends)
  - Alert configuration for failures
  - Historical performance data

#### Acceptance Criteria
- [ ] Cron jobs execute on schedule
- [ ] Batch processing reduces API call volume
- [ ] Performance metrics visible in dashboard
- [ ] Monitoring alerts trigger on failures
- [ ] Execution costs tracked and displayed

---

### 9. Testing and Integration (15 hours)

#### Description
End-to-end testing for all admin workflows including edge cases such as maximum competitor limits and sync failures. Integrate with overall backend systems and ensure proper error handling.

#### Functional Requirements
- **Test Coverage**
  - Unit tests for all components
  - Integration tests for AWS service interactions
  - End-to-end user flow tests
  - Edge case scenarios:
    - Maximum competitor limit (20)
    - Sync failures and recovery
    - Concurrent user actions
    - Permission boundary testing

- **Integration Points**
  - AWS Cognito authentication
  - DynamoDB data operations
  - OpenSearch queries
  - EventBridge scheduling
  - SQS queue operations
  - CloudWatch logging
  - SES email delivery

- **Quality Assurance**
  - Automated test suite
  - Manual QA testing checklist
  - Performance benchmarking
  - Security testing (OWASP)

#### Acceptance Criteria
- [ ] All admin flows tested end-to-end
- [ ] Edge cases handled gracefully
- [ ] Integration tests pass for all AWS services
- [ ] Performance meets defined benchmarks
- [ ] Security vulnerabilities addressed
- [ ] Test coverage > 80%

---

## Marketing Manager Persona Specification

**Total Hours**: 280 hours

### Overview
The Marketing Manager Persona focuses on competitive analysis, reporting, and AI-driven insights. This role enables data-driven decision making through comprehensive review analysis, comparative reports, and actionable recommendations. The system supports up to 20 retailers per analysis with dynamic filtering, drill-downs, and automated PDF report generation.

### 1. Login and Analysis Scope Selection (30 hours)

#### Description
Develop role-restricted login; create dashboard with overview cards (shopping center on top, retailers below); enable selection of retailer/center and view dynamic competitor lists (up to 20).

#### Functional Requirements
- **Authentication**
  - Role-restricted access (Marketing Manager persona)
  - AWS Cognito integration
  - Session management

- **Dashboard Overview**
  - Card-based layout:
    - Shopping centers (top section)
    - Retailers (below shopping centers)
  - Entity cards display:
    - Name and location
    - Competitor count
    - Last analysis date
    - Quick view metrics

- **Entity Selection**
  - Click card to select entity
  - Display dynamic competitor list (up to 20)
  - Competitor details:
    - Name
    - Location
    - Data availability status
    - Last sync date

#### Acceptance Criteria
- [ ] Marketing Manager can login successfully
- [ ] Dashboard displays shopping centers at top
- [ ] Retailers displayed below shopping centers
- [ ] Select entity to view competitor list
- [ ] Up to 20 competitors displayed per entity
- [ ] Competitor data status visible

---

### 2. Time Period and Competitor Configuration (40 hours)

#### Description
Integrate radio buttons for 1/2/3-year filters (backend query updates in OpenSearch); enable dynamic addition of competitors (input name/location, queue Lambda for ingestion/analysis); add progress notifications.

#### Functional Requirements
- **Time Period Filters**
  - Radio button selection: 1 year, 2 years, 3 years
  - Filter applies to all analysis views
  - Backend OpenSearch query updates
  - Date range calculation based on selection
  - Visual indication of active filter

- **Dynamic Competitor Addition**
  - "Add Competitor" button
  - Input form:
    - Business name
    - Location/address
    - Google Places API search integration
  - Queue competitor for ingestion via Lambda
  - SQS message for data collection

- **Progress Notifications**
  - Toast/banner notifications for:
    - Competitor added to queue
    - Ingestion started
    - Ingestion completed
    - Ingestion failed (with retry option)
  - Progress indicator for active ingestions
  - Estimated completion time

#### Acceptance Criteria
- [ ] Time period radio buttons (1/2/3 years)
- [ ] Filter updates all analysis views
- [ ] OpenSearch queries filter by date range
- [ ] Add competitor via search form
- [ ] Competitor queued to Lambda/SQS
- [ ] Progress notifications display correctly
- [ ] User notified on completion/failure

---

### 3. Comparative Analysis and Drill-Downs (55 hours)

#### Description
Develop table views for categories (e.g., averages vs. competitors); add detailed clicks showing mention counts, score distributions (e.g., pie charts for Poor/Average/Good/Excellent), sentiment summaries, and RAG insights from Bedrock. Support drill-downs for volume + positive/negative breakdowns.

#### Functional Requirements
- **Table View Structure**
  - Categories as rows (e.g., Professionalism, Price, Quality, Ambiance)
  - Entity and competitors as columns
  - Average scores displayed in cells
  - Color coding for performance (red/yellow/green)
  - **Hide columns with zero reviews** (no data displayed for competitors without reviews)
  - Show **review count per attribute** (e.g., "5 of 10 reviews mention this attribute")
  - Display **combined review totals across all competitors** for each topic/category

- **Category Metrics**
  - Average score per category
  - Comparison to competitor averages
  - Trend indicators (up/down/stable)
  - Mention count per category
  - Count of reviews per attribute displayed

- **Sorting and Prioritization**
  - **Dual sorting options**:
    - Sort by **overall popularity** (based on combined review counts across competitors)
    - Sort by **importance to specific retailer**
  - Toggle between sorting views for better insights
  - User-selectable sorting preference

- **Drill-Down Details**
  - Click category cell to view details:
    - Total mention count
    - Score distribution (pie/bar chart) - **Four-level rating system**:
      - **Poor**
      - **Average**
      - **Good**
      - **Excellent**
    - Sentiment breakdown (positive/negative/neutral)
    - Sample reviews for each rating tier
  - **Expand and collapse detailed view per retailer**
  - Optional **'More Details' button** to drill down into specific attributes and review summaries

- **AI Insights Integration**
  - RAG (Retrieval-Augmented Generation) insights from Amazon Bedrock
  - Category-specific insights:
    - Key strengths
    - Areas for improvement
    - Competitive advantages
    - Trending themes
  - Sentiment summaries with context

- **Volume and Sentiment Drill-Downs**
  - Review volume over time graphs
  - Positive vs. negative mention trends
  - Category performance evolution
  - Competitor comparison overlays

#### Acceptance Criteria
- [ ] Table displays categories vs. competitors
- [ ] Columns with zero reviews are hidden
- [ ] Review counts per attribute displayed
- [ ] Combined review totals shown per category
- [ ] Dual sorting options implemented (popularity and importance)
- [ ] Average scores calculated correctly
- [ ] Click category to drill down
- [ ] Mention counts displayed
- [ ] Score distribution visualized with 4-level rating (Poor/Average/Good/Excellent)
- [ ] Sentiment breakdown shown
- [ ] Expand/collapse detailed view per retailer
- [ ] 'More Details' button for attribute drill-down
- [ ] RAG insights from Bedrock integrated
- [ ] Volume trends displayed over time
- [ ] Positive/negative breakdowns visible

---

### 4. Reporting Generation and Scheduling (50 hours)

#### Description
Implement "Generate Report" button for PDFs (executive summary, comparisons, insights, recommendations with distributions and time-filters); add scheduling options (weekly emails via SES); enable immediate send/download and historical report library. Restructure for readability (category rows vs. competitor columns, exclude low-data categories). Automated PDF reports emailed to marketing managers and retailers with configurable weekly/monthly scheduling per retailer. No retailer portal in MVP; PDF is the primary delivery method.

#### Functional Requirements
- **Report Generation**
  - "Generate Report" button on dashboard
  - PDF output with sections:
    - Executive summary
    - Entity overview
    - Category comparisons (rows: categories, columns: competitors)
    - Score distributions by category
    - Time-filtered insights
    - AI-generated recommendations
    - Methodology appendix

- **Report Structure (Restructured)**
  - Category rows vs. competitor columns format
  - Exclude categories with insufficient data
  - Highlight top 3 performing categories
  - Highlight bottom 3 requiring attention
  - Visual charts and graphs
  - Professional formatting with branding

- **Scheduling Options**
  - Weekly email delivery via SES
  - Configure delivery day and time
  - Select recipients
  - Email body customization
  - Attachment settings

- **Immediate Actions**
  - Send now button
  - Download PDF directly
  - Preview before sending
  - Email delivery confirmation

- **Historical Report Library**
  - List of all generated reports
  - Filter by date, entity, type
  - Download archived reports
  - Delete old reports
  - Storage in S3 with lifecycle policies

#### Acceptance Criteria
- [ ] Generate PDF report on demand
- [ ] Report includes all required sections
- [ ] Category rows vs. competitor columns layout
- [ ] Low-data categories excluded
- [ ] Schedule weekly reports
- [ ] Send report immediately via email
- [ ] Download PDF directly
- [ ] Access historical report library
- [ ] Filter and search archived reports
- [ ] Reports stored in S3

---

### 5. AI Insights and Transparency (35 hours)

#### Description
Display AI-generated categories (e.g., professionalism, price) with volumes, averages across competitors; show credibility details (e.g., how scores are derived); support future LLM plugging (e.g., Bedrock, others). LLM makes autonomous decisions on attribute visibility and ratings. Four-level rating system: Excellent, Good, Average, and Poor. Ratings determined by LLM analysis of review sentiment and content.

#### Functional Requirements
- **AI-Generated Categories**
  - Dynamic category detection from reviews using LLM
  - Common categories:
    - Professionalism
    - Price/Value
    - Quality
    - Customer Service
    - Ambiance
    - Cleanliness
    - Speed/Efficiency
  - Custom categories based on review content
  - **MVP Decision Logic**: LLM makes autonomous decisions on attribute visibility and ratings
  - Future phases will add human-in-the-loop capability to manually hide fields with zero reviews or adjust categorization

- **Category Metrics Display**
  - Total mention volume per category
  - Average score across competitors
  - Entity-specific average
  - Competitor comparison
  - Statistical significance indicators

- **Credibility and Transparency**
  - "How is this calculated?" tooltips
  - Methodology explanation:
    - Review source and volume
    - AI model used (Bedrock)
    - Scoring algorithm
    - Confidence levels
  - Sample reviews supporting scores
  - Outlier detection and handling

- **LLM Integration Architecture**
  - Abstraction layer for LLM provider
  - Support for Amazon Bedrock (primary)
  - Pluggable architecture for future LLMs:
    - OpenAI GPT
    - Anthropic Claude
    - Google PaLM
    - Custom models
  - Configuration for model selection
  - Response format standardization

#### Acceptance Criteria
- [ ] AI generates categories from review content
- [ ] Display category volumes and averages
- [ ] Show competitor comparisons per category
- [ ] Credibility details accessible via tooltips
- [ ] Methodology clearly explained
- [ ] Sample reviews link to scores
- [ ] LLM abstraction layer implemented
- [ ] Amazon Bedrock integrated
- [ ] Architecture supports future LLM swapping

---

### 6. Ad-Hoc Analysis and Feedback Features (25 hours)

#### Description
Enable on-the-fly comparisons (tenant vs. 1-3 competitors); add thumbs up/down feedback on insights for improvements. Simple feedback UI with three options: Agree, Disagree, or Neutral for each AI recommendation. Option for free-text comments to help train and improve the LLM. Initial implementation kept minimal with plans to expand in future phases.

#### Functional Requirements
- **Ad-Hoc Comparison Tool**
  - Select primary entity (tenant)
  - Choose 1-3 competitors for comparison
  - Generate instant comparison report
  - Categories included:
    - All available categories
    - User-selected subset
  - Side-by-side visualization
  - Export to PDF/CSV
  - **Enable dynamic competitor selection for flexible analysis scenarios**

- **Comparison Features**
  - Radar/spider charts for multi-category view
  - Bar charts for category-by-category
  - Highlight significant differences
  - Statistical significance testing
  - Trend analysis over time

- **Feedback Mechanism (Simplified for MVP)**
  - **Three-option feedback**: Agree, Disagree, or Neutral for each AI recommendation
  - Optional free-text comments field
  - Feedback storage for LLM training and improvement
  - Aggregate feedback analytics for admins
  - Initial implementation kept minimal
  - Plans to expand feedback features in future phases

- **Improvement Loop**
  - Track feedback trends
  - Identify low-confidence categories
  - Flag insights for review
  - A/B testing for insight improvements

#### Acceptance Criteria
- [ ] Select tenant and 1-3 competitors
- [ ] Generate ad-hoc comparison
- [ ] Dynamic competitor selection enabled
- [ ] Visualizations display correctly
- [ ] Export comparison to PDF/CSV
- [ ] Three-option feedback (Agree/Disagree/Neutral) implemented
- [ ] Optional free-text comments available
- [ ] Feedback stored for LLM training
- [ ] Admins can view feedback analytics

---

### 7. Wireframes and Reporting Format Refinement (20 hours)

#### Description
Update marketing dashboard wireframes per feedback (e.g., timeframe clarity, comparative drill-downs); define PDF format with cut-offs for visibility. PDF reports structured with categories as rows and competitors as columns for easy comparison. Exclude categories with insufficient review data to maintain report clarity.

#### Functional Requirements
- **Wireframe Updates**
  - Timeframe filter prominence
  - Comparative drill-down navigation
  - Category table layout
  - Report generation workflow
  - Mobile responsiveness considerations

- **Feedback Integration**
  - Stakeholder review sessions
  - UI/UX iterations
  - Clarify drill-down interactions
  - Simplify complex views

- **PDF Report Format Definition**
  - Page layout and margins
  - Font sizes and hierarchy
  - Chart sizes and positioning
  - Category visibility cut-offs:
    - Minimum mention count threshold
    - Minimum competitor coverage
  - Table formatting for readability
  - Section breaks and navigation

- **Design Deliverables**
  - High-fidelity wireframes
  - Interactive prototypes
  - PDF template mockups
  - Style guide documentation

#### Acceptance Criteria
- [ ] Wireframes updated per feedback
- [ ] Timeframe selection clearly visible
- [ ] Drill-down navigation intuitive
- [ ] PDF format defined with cut-offs
- [ ] Category visibility thresholds set
- [ ] Stakeholder approval obtained
- [ ] Responsive design validated

---

### 8. Testing and Integration (25 hours)

#### Description
End-to-end testing for marketing flows, including ad-hoc scenarios and report exports; ensure scalability for future sources.

#### Functional Requirements
- **Test Coverage**
  - Unit tests for all components
  - Integration tests for:
    - OpenSearch queries
    - Bedrock AI insights
    - PDF generation
    - Email delivery
  - End-to-end user flow tests
  - Ad-hoc analysis scenarios

- **Edge Cases**
  - No data available for entity
  - Insufficient competitor data
  - Time period with sparse reviews
  - Maximum competitor limit (20)
  - Concurrent report generation
  - Large dataset performance

- **Scalability Testing**
  - Future data source integration readiness
  - Performance with growing data volumes
  - Concurrent user load testing
  - Report generation at scale

- **Quality Assurance**
  - Automated test suite
  - Manual QA checklist
  - Cross-browser testing
  - Accessibility compliance
  - Performance benchmarking

#### Acceptance Criteria
- [ ] All marketing flows tested end-to-end
- [ ] Ad-hoc analysis scenarios validated
- [ ] Report export tested (PDF, email)
- [ ] Edge cases handled gracefully
- [ ] Scalability validated for future sources
- [ ] Performance meets benchmarks
- [ ] Test coverage > 80%
- [ ] Accessibility standards met

---

### Key Features and Enhancements

This section provides additional context and detailed specifications for Marketing Manager Persona features that span multiple tasks.

#### Data Presentation and Filtering
- **Hide columns with zero reviews** in dashboard tables
  - No data displayed for competitors without reviews
  - Cleaner, more focused analysis views
  - Automatic filtering based on data availability
- **Display count of reviews per attribute**
  - Example: "5 of 10 reviews mention this attribute"
  - Provides context for score reliability
  - Helps users assess data confidence
- **Show combined review totals across all competitors** for each topic or category
  - Aggregate view of category importance
  - Industry-level insights
  - Benchmark against market trends

#### Competitor Management
- **Support listing of 20 retailers** with all reviewed categories displayed
  - Maximum 20 competitors per entity
  - Complete category coverage
  - Comprehensive comparative analysis
- **Show comprehensive competitor analysis** across all attributes
  - Side-by-side comparisons
  - Performance benchmarking
  - Identify competitive gaps and opportunities

#### Sorting and Prioritization
- **Implement dual sorting options**:
  1. **Sort by overall popularity** (based on combined review counts across competitors)
     - Industry-wide importance
     - Most discussed attributes
     - Market-level priorities
  2. **Sort by importance to specific retailer**
     - Entity-specific relevance
     - Customized priorities
     - Retailer-centric view
- **Enable users to toggle between these views** for better insights
  - Quick switching mechanism
  - User preference persistence
  - Flexible analysis approach

#### Rating System
- **Four-level rating system**: Excellent, Good, Average, and Poor
  - Simplified from 5-level scale for clarity
  - Clear differentiation between levels
  - Easy-to-understand performance tiers
- **Ratings determined by LLM analysis** of review sentiment and content
  - Automated sentiment analysis
  - Natural language processing
  - Contextual understanding of review content
  - Amazon Bedrock integration

#### Detailed View Options
- **Expand and collapse detailed view per retailer**
  - Space-efficient design
  - User-controlled detail level
  - Focus on relevant information
- **Optional 'More Details' button** to drill down into specific attributes and review summaries
  - Deep-dive capability
  - Review-level granularity
  - Sample review access
  - Full context availability

#### Feedback Loop and AI Training
- **Simple feedback UI with three options**: Agree, Disagree, or Neutral for each AI recommendation
  - Quick feedback mechanism
  - Minimal user effort required
  - Clear opinion capture
- **Option for free-text comments** to help train and improve the LLM
  - Detailed feedback capability
  - Contextual improvement suggestions
  - User voice captured
- **Initial implementation kept minimal** with plans to expand in future phases
  - MVP-focused scope
  - Foundation for future enhancements
  - Scalable architecture

#### PDF Report Generation and Distribution
- **Automated PDF reports emailed** to marketing managers and retailers
  - Scheduled delivery
  - Consistent reporting cadence
  - Professional formatting
- **Schedule configurable as weekly or monthly** via checkbox per retailer
  - Flexible scheduling
  - Entity-specific configuration
  - Customizable delivery frequency
- **Email distribution lists stored per shop** for automated delivery
  - Per-retailer recipient management
  - Unrestricted recipient count
  - Centralized configuration
- **No retailer portal in MVP**; PDF is the primary delivery method
  - Simplified distribution model
  - Email-based access
  - Future portal capability planned

#### Report Format Structure
- **PDF reports structured with categories as rows and competitors as columns** for easy comparison
  - Intuitive layout
  - Quick visual comparison
  - Category-focused analysis
- **Exclude categories with insufficient review data** to maintain report clarity
  - Data quality threshold
  - Clean, focused reports
  - Avoid noise from sparse data

---

### Technical Implementation Details

This section outlines the technical architecture and implementation approach for the Marketing Manager Persona.

#### Data Collection Pipeline
- **Pull Google Place ID from Precision Group APA endpoints**
  - Integration with Precision Group systems
  - Entity identification
  - Standardized place references
  - API endpoint connectivity
- **Use SERP API (free trial for MVP)** to fetch review KPIs and attributes
  - Google Reviews data source
  - Review metadata extraction
  - Rating and sentiment data
  - Free tier for initial MVP
  - Scalable to paid tier for production
- **LLM processes collected data** and stores results in database
  - Amazon Bedrock for AI processing
  - Category extraction
  - Sentiment analysis
  - Rating generation
  - DynamoDB/OpenSearch storage

#### MVP Decision Logic
- **LLM makes autonomous decisions** on attribute visibility and ratings
  - Automated categorization
  - No manual intervention required in MVP
  - AI-driven insights
  - Consistent scoring methodology
- **Future phases will add human-in-the-loop capability**
  - Manual field hiding option
  - Category adjustment controls
  - Admin override capability
  - Supervised learning improvements

#### Time Period Filtering
- **Support for 1, 2, and 3-year time period selections**
  - Radio button interface
  - User-selectable time ranges
  - Historical trend analysis
- **Backend query updates in OpenSearch** to filter data by selected time range
  - Dynamic query generation
  - Date-based filtering
  - Optimized performance
  - Real-time updates

#### Comparative Analysis Views
- **Table views showing category averages versus competitors**
  - Matrix layout
  - Color-coded performance
  - Quick visual comparison
- **Detailed drill-downs displaying**:
  - Mention counts per category
  - Score distributions (pie charts for Poor, Average, Good, Excellent)
  - Sentiment summaries
  - RAG insights from Amazon Bedrock
  - Sample review excerpts
  - Volume trends over time
  - Positive/negative breakdowns

#### Ad-Hoc Analysis Capability
- **Enable on-the-fly comparisons** between tenant and 1-3 selected competitors
  - Real-time analysis
  - User-defined comparison sets
  - Flexible selection
- **Support dynamic competitor selection** for flexible analysis scenarios
  - Change competitors without configuration
  - Instant report regeneration
  - Multiple comparison scenarios
  - Export to PDF/CSV

#### Data Sources and APIs
- **Precision Group APA Endpoints**: Entity data and Google Place IDs
- **SERP API**: Google Reviews collection (free trial for MVP)
- **Google Places API**: Competitor search and details
- **Amazon Bedrock**: LLM processing and RAG insights
- **AWS OpenSearch**: Review data storage and querying
- **AWS DynamoDB**: Entity and competitor metadata
- **AWS Lambda**: Serverless processing functions
- **AWS SES**: Email delivery for reports
- **AWS S3**: Report storage and archival

#### AI and Machine Learning
- **Amazon Bedrock** as primary LLM provider
  - Natural language understanding
  - Category extraction
  - Sentiment analysis
  - Rating generation
  - Insight recommendations
- **Pluggable LLM architecture** for future flexibility
  - Support for multiple LLM providers
  - Easy switching between models
  - Comparative testing capability
- **Retrieval-Augmented Generation (RAG)**
  - Context-aware insights
  - Review-grounded recommendations
  - Factual accuracy
  - Source attribution

---

## Technical Architecture Notes

### AWS Services Integration
- **Authentication**: AWS Cognito User Pools
- **Database**: DynamoDB for entities/competitors
- **Search**: OpenSearch for review data and analytics
- **Scheduling**: EventBridge for cron jobs
- **Queuing**: SQS for async ingestion tasks
- **Compute**: Lambda for serverless functions
- **ETL**: Glue for data transformation
- **AI**: Bedrock for insights generation
- **Email**: SES for report delivery
- **Storage**: S3 for reports and configuration
- **Monitoring**: CloudWatch for logs and metrics

### Security Considerations
- Role-based access control (RBAC)
- Multi-factor authentication (MFA)
- Encrypted data at rest and in transit
- API key secure storage
- Audit logging for compliance
- Session management and timeout
- OWASP security best practices

### Performance Requirements
- Dashboard load time < 2 seconds
- Analysis drill-down < 1 second
- Report generation < 30 seconds
- Email delivery < 5 minutes
- API response time < 500ms
- Support 50+ concurrent users

### Future Scalability
- Multi-data source architecture (beyond Google Reviews)
- Additional LLM provider support
- Enhanced category customization
- Mobile application support
- Advanced analytics and ML models
- Multi-tenant architecture readiness

---

## Glossary

- **Entity**: A retailer or shopping center being analyzed
- **Competitor**: A business compared against the primary entity
- **Category**: A dimension of analysis (e.g., Professionalism, Price)
- **RAG**: Retrieval-Augmented Generation (AI technique)
- **MFA**: Multi-Factor Authentication
- **RBAC**: Role-Based Access Control
- **ETL**: Extract, Transform, Load (data processing)
- **SES**: Simple Email Service (AWS)
- **SQS**: Simple Queue Service (AWS)

---

## Document Control

**Version**: 2.0
**Created**: 2025-10-23
**Last Updated**: 2025-10-23
**Status**: Revised Based on Updated Requirements
**Approvers**: Product Owner, Tech Lead, UX Lead

### Revision History
- **v1.0** (2025-10-23): Initial specification document
- **v2.0** (2025-10-23): Updated with revised requirements
  - Added effort hours (Admin: 240h, Marketing Manager: 280h)
  - Changed rating system from 5-level to 4-level (Poor, Average, Good, Excellent)
  - Added Key Features and Enhancements section for Marketing Manager persona
  - Added Technical Implementation Details section
  - Updated feedback mechanism to Agree/Disagree/Neutral
  - Added data presentation filtering and sorting details
  - Included SERP API and Precision Group APA integration details
  - Clarified per-retailer recipient configuration
  - Added MVP decision logic and human-in-the-loop future plans
