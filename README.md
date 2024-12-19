# Timesheet Reminder Email Automation

## Overview
This automation helps GVSU Parking Services maintain consistent timesheet submissions by automatically sending reminder emails on a biweekly schedule. Using a single event transform, it combines time checking, scheduling logic, and email formatting.

## Technical Implementation

### Event Transform Conditions
```javascript
// Time Window Check
current_hour >= 8 && current_hour < 9

// Friday Check
current_day == "Friday"

// Biweekly Pattern Check
// If Week 1, next email Week 3
is_biweekly_friday == true
```

### Email Template Structure
```html
<html>
<body style='font-family: Arial, sans-serif; line-height: 1.6;'>
    <p>Dear Parking Services Team,</p>
    
    <p>This is a friendly reminder to submit your hours.</p>
    
    <p>Important details:</p>
    <ul style='margin-left: 20px;'>
        <li>Submit hours through Workday</li>
        <li>Deadline is 4 PM</li>
        <li>Hours should be accurate</li>
        <li>Failure of hours submission will delay in payroll processing</li>
    </ul>
    
    <p style='margin-top: 20px;'>Best regards,<br>
    GVSU PARKING SERVICES</p>
</body>
</html>
```

### Transform Output Types
1. When conditions match:
   ```json
   {
     "subject": "Timesheet Submission Reminder",
     "body": "HTML_FORMATTED_EMAIL",
     "send_time": "CURRENT_TIMESTAMP"
   }
   ```

2. When conditions don't match:
   ```json
   {
     "message": "Not time to send reminder yet"
   }
   ```

## Testing Procedures

### Pre-Deployment Tests
1. Time Window Test:
   - Test before 8:00 AM (should not send)
   - Test between 8:00-9:00 AM (should send)
   - Test after 9:00 AM (should not send)

2. Day Check Test:
   - Test on Friday (should process)
   - Test on other days (should return "not time" message)

3. Biweekly Pattern Test:
   - Week 1: Initial send
   - Week 2: No send
   - Week 3: Should send
   - Week 4: No send

### Email Format Testing
1. HTML Rendering:
   - Check font application
   - Verify list formatting
   - Confirm line spacing
   - Test across email clients

2. Content Verification:
   - Subject line presence
   - All bullet points included
   - Signature formatting
   - No HTML tag visibility

## Automation Schedule
- Initial Trigger: Starts on first Friday run
- Subsequent Runs: 
  - If started Week 1 Friday → Next email Week 3 Friday
  - Continues every other Friday indefinitely
- Time Window: 8:00 AM - 9:00 AM

## Implementation Files
- `timesheet_reminder.json`: 
  ```json
  {
    "version": "1.0",
    "transform_type": "event",
    "conditions": {
      "time_check": true,
      "day_check": true,
      "biweekly_check": true
    },
    "email_template": "HTML_CONTENT",
    "output_format": "JSON"
  }
  ```

## Troubleshooting Guide
1. Email Not Sending:
   - Verify current time is within 8:00-9:00 AM window
   - Confirm it's a Friday
   - Check biweekly pattern alignment

2. Format Issues:
   - Verify HTML syntax is intact
   - Check for missing closing tags
   - Confirm style attributes

3. Schedule Problems:
   - Review initial run date
   - Verify biweekly calculation
   - Check timezone settings

## Maintenance
The automation is self-maintaining but may need updates for:
- Email content modifications
- Time window adjustments
- HTML template updates
- Biweekly pattern changes

## Version History
- 1.0: Initial implementation
- Current Version: 1.0
  - Features: Time check, biweekly scheduling, HTML email

## Future Enhancements (Optional)
1. Add confirmation logging
2. Implement error reporting
3. Add email delivery tracking
4. Create backup send window

## Support
For issues or modifications, contact:
- Automation Team
- IT Support
