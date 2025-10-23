# Admin & Marketing Manager Dashboard Specification

## Document Overview
This specification outlines the functional requirements for two primary user personas:
1. **Admin Persona** - System administration, configuration, and monitoring
2. **Marketing Manager Persona** - Analysis, reporting, and competitive insights

---

## Admin Persona Specification

### 1. Login and Dashboard Overview

#### Description
Develop a secure authentication system integrated with AWS Cognito, supporting Multi-Factor Authentication (MFA). The dashboard provides a comprehensive overview of system health and operational metrics.

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

### 2. Data Sources and Schedules Management

#### Description
Create a comprehensive interface for viewing and configuring scheduled data collection jobs. Initially focused on Google Reviews ingestion via AWS Glue ETL with fixed schedules.

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

### 3. Entities & Competitors Management

#### Description
Build management interface for retailers and shopping centers with dynamic competitor tracking capabilities. Support up to 20 competitors per entity with Google API integration.

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

### 4. Reporting and Email Configuration

#### Description
Develop settings interface for automated report generation and distribution. Support scheduled PDF reports with customizable templates, delivered via AWS SES.

#### Functional Requirements
- **Report Scheduling**
  - Configure weekly/monthly report schedules
  - Select report recipients (unrestricted email list)
  - Set delivery time and timezone
  - Enable/disable scheduled reports

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

### 5. User Management and Monitoring

#### Description
Create user administration interface with basic AWS Cognito User Pool integration. Support user lifecycle management and audit logging.

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

### 6. Wireframes and Feedback Integration

#### Description
Refine and update admin dashboard wireframes based on stakeholder feedback, focusing on system health views, error logs, and future retailer access.

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

### 7. AI Rating Methodology Exposure

#### Description
Expose AI rating scale and methodology in admin views. This task focuses on the administrative visibility aspect; detailed LLM logic is covered in Marketing Manager persona.

#### Functional Requirements
- **Rating Scale Display**
  - Expose 5-point scale: Poor → Below Average → Average → Good → Excellent
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

### 8. Scheduled Data Collection Optimization

#### Description
Implement optimized cron jobs for data collection with performance enhancements. Monitor execution and expose metrics in dashboard.

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

### 9. Testing and Integration

#### Description
Comprehensive end-to-end testing for all admin flows, including edge cases and integration with backend services.

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

### 1. Login and Analysis Scope Selection

#### Description
Develop role-restricted login with dashboard overview featuring shopping centers and retailers. Enable entity selection and display associated competitor lists.

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

### 2. Time Period and Competitor Configuration

#### Description
Integrate time period filters and enable dynamic competitor addition with backend query support and progress notifications.

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

### 3. Comparative Analysis and Drill-Downs

#### Description
Build comprehensive table views for category-based comparative analysis with detailed drill-down capabilities, sentiment analysis, and AI insights.

#### Functional Requirements
- **Table View Structure**
  - Categories as rows (e.g., Professionalism, Price, Quality, Ambiance)
  - Entity and competitors as columns
  - Average scores displayed in cells
  - Color coding for performance (red/yellow/green)

- **Category Metrics**
  - Average score per category
  - Comparison to competitor averages
  - Trend indicators (up/down/stable)
  - Mention count per category

- **Drill-Down Details**
  - Click category cell to view details:
    - Total mention count
    - Score distribution (pie/bar chart):
      - Poor
      - Below Average
      - Average
      - Good
      - Excellent
    - Sentiment breakdown (positive/negative/neutral)
    - Sample reviews for each rating tier

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
- [ ] Average scores calculated correctly
- [ ] Click category to drill down
- [ ] Mention counts displayed
- [ ] Score distribution visualized (pie/bar charts)
- [ ] Sentiment breakdown shown
- [ ] RAG insights from Bedrock integrated
- [ ] Volume trends displayed over time
- [ ] Positive/negative breakdowns visible

---

### 4. Reporting Generation and Scheduling

#### Description
Implement comprehensive PDF report generation with executive summaries, comparisons, and scheduling capabilities. Reports delivered via AWS SES with historical library access.

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

### 5. AI Insights and Transparency

#### Description
Display AI-generated categories with volumes and averages, ensuring transparency in scoring methodology. Support future LLM integration flexibility.

#### Functional Requirements
- **AI-Generated Categories**
  - Dynamic category detection from reviews
  - Common categories:
    - Professionalism
    - Price/Value
    - Quality
    - Customer Service
    - Ambiance
    - Cleanliness
    - Speed/Efficiency
  - Custom categories based on review content

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

### 6. Ad-Hoc Analysis and Feedback Features

#### Description
Enable on-the-fly comparative analysis and user feedback mechanisms to improve AI insights quality.

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

- **Comparison Features**
  - Radar/spider charts for multi-category view
  - Bar charts for category-by-category
  - Highlight significant differences
  - Statistical significance testing
  - Trend analysis over time

- **Feedback Mechanism**
  - Thumbs up/down on AI insights
  - Feedback form for incorrect insights:
    - What was wrong?
    - Expected insight
    - Additional context
  - Feedback storage for model improvement
  - Aggregate feedback analytics for admins

- **Improvement Loop**
  - Track feedback trends
  - Identify low-confidence categories
  - Flag insights for review
  - A/B testing for insight improvements

#### Acceptance Criteria
- [ ] Select tenant and 1-3 competitors
- [ ] Generate ad-hoc comparison
- [ ] Visualizations display correctly
- [ ] Export comparison to PDF/CSV
- [ ] Thumbs up/down on insights
- [ ] Submit detailed feedback on incorrect insights
- [ ] Feedback stored for analysis
- [ ] Admins can view feedback analytics

---

### 7. Wireframes and Reporting Format Refinement

#### Description
Update marketing dashboard wireframes based on feedback, clarifying timeframe selections and comparative drill-downs. Define PDF report format with visibility optimizations.

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

### 8. Testing and Integration

#### Description
Comprehensive end-to-end testing for marketing manager flows, including ad-hoc scenarios, report exports, and scalability validation.

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

**Version**: 1.0
**Created**: 2025-10-23
**Last Updated**: 2025-10-23
**Status**: Draft for Review
**Approvers**: Product Owner, Tech Lead, UX Lead
