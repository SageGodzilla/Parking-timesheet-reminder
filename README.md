# Timesheet Reminder Email Automation using TINES

## Overview
This automation helps maintain consistent timesheet submissions by automatically sending reminder emails on a biweekly schedule during morning hours on Fridays. Using event transform with timezone handling, it ensures accurate delivery timing.

## Technical Implementation

### Event Transform Conditions
```python
from datetime import datetime, timezone
import pytz

# Time Window Check (Morning Hours)
eastern = pytz.timezone('America/Detroit')
now = datetime.now(eastern)
current_hour = now.hour
current_day = now.weekday()

# Morning Hours Check (0-11 AM)
is_morning = 0 <= current_hour <= 11

# Friday Check
is_friday = current_day == 4
```

### Email Template Structure
```html
<html>
<body style='font-family: Arial, sans-serif; line-height: 1.6;'>
    <p>Dear Team,</p>
    
    <p>This is a friendly reminder to submit your hours.</p>
    
    <p>Important details:</p>
    <ul style='margin-left: 20px;'>
        <li>Submit hours through Workday</li>
        <li>Deadline is 4 PM</li>
        <li>Record your hours accurately</li>
        <li>Failure of submission will delay in payroll processing</li>
    </ul>
    
    <p style='margin-top: 20px;'>Best,<br>
     SERVICES</p>
</body>
</html>
```

### Transform Output Types
1. When conditions match:
```json
{
    "subject": "Timesheet Submission Reminder",
    "body": "HTML_FORMATTED_EMAIL"
}
```

2. When conditions don't match:
```python
f"Current hour is {current_hour} - Should be between 0 and 11 for morning hours"
# or
"Not Friday - no reminder needed"
```

## Testing Procedures

### Pre-Deployment Tests
1. Time Window Test:
   * Test in morning hours (should send)
   * Test in afternoon hours (should not send)
2. Day Check Test:
   * Test on Friday (should process)
   * Test on other days (should return "Not Friday" message)

### Email Format Testing
1. HTML Rendering:
   * Check font application
   * Verify list formatting
   * Confirm line spacing
2. Content Verification:
   * Subject line presence
   * All bullet points included
   * Signature formatting

## Automation Schedule
* Runs during morning hours (before noon)
* Executes on Fridays
* Uses Eastern timezone (America/Detroit)

## Technical Notes
* Uses pytz library for timezone handling
* Implements 24-hour format for time checking
* Handles local time conversion automatically

## Maintenance
The automation may need updates for:
* Email content modifications
* Time window adjustments
* Timezone changes
* HTML template updates

## Version History
* 1.0: Initial implementation with basic time check
* 1.1: Added timezone support
* Current Version: 1.1
   * Features: Timezone awareness, morning hours check, HTML email

## Troubleshooting Guide
1. Time Issues:
   * Verify timezone settings
   * Check server time vs local time
   * Confirm morning hours detection
2. Format Issues:
   * Verify HTML syntax
   * Check email rendering
   * Confirm content formatting
