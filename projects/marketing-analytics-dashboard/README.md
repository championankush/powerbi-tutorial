# Marketing Campaign Analytics Dashboard 📣

## Project Overview

This dashboard demonstrates PowerBI solutions for marketing, focusing on campaign performance, lead generation, and ROI analytics.

## Key Features
- **Campaign Performance**: Impressions, clicks, conversions, cost
- **Lead Analytics**: Source, stage, conversion rate
- **ROI Analysis**: Cost per lead, cost per acquisition, campaign ROI
- **Attribution**: Multi-touch, channel effectiveness

## Data Model
### Tables
- **Campaigns**: CampaignID, Name, StartDate, EndDate, Channel, Budget, Status
- **Leads**: LeadID, CampaignID, Source, Date, Stage, Converted
- **Costs**: CostID, CampaignID, Date, Amount, Type
- **Conversions**: ConversionID, LeadID, Date, Value
- **Channels**: ChannelID, Name, Type

## DAX Examples
```dax
// Conversion Rate
Conversion Rate = DIVIDE(
    COUNTROWS(FILTER(Leads, Leads[Converted] = TRUE())),
    COUNTROWS(Leads), 0)

// Cost per Lead
Cost per Lead = DIVIDE(
    SUM(Costs[Amount]),
    COUNTROWS(Leads), 0)

// Campaign ROI
Campaign ROI = DIVIDE(
    SUM(Conversions[Value]) - SUM(Costs[Amount]),
    SUM(Costs[Amount]), 0)

// Channel Effectiveness
Channel Effectiveness = RANKX(
    ALL(Channels),
    [Conversion Rate],, DESC)
```

## Visualizations
- **KPI Cards**: Conversion rate, cost per lead, ROI
- **Trend Charts**: Leads over time, campaign spend
- **Bar/Pie Charts**: Channel performance, conversion by source
- **Attribution Analysis**: Multi-touch funnel
- **Campaign Comparison**: ROI by campaign

## Best Practices
- Integrate with CRM/marketing automation
- Use RLS for campaign manager access
- Monitor real-time campaign performance
- Analyze attribution models

## Sample Data
See `examples/sample-data/marketing-campaigns.csv` (create as needed)

## Resources
- [PowerBI for Marketing](https://docs.microsoft.com/en-us/power-bi/solutions/industry-marketing)

---

**Maximize marketing impact with actionable campaign analytics!** 📣 