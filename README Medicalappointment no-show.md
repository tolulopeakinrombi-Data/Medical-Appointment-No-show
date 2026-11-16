# Medical Appointment No-Shows Analysis

**Why do patients miss their medical appointments?** An end-to-end data analysis project using Excel, Power Query and Power BI on 110,521 appointment records.

![Dashboard preview](images/dashboard.png)

## Project overview

Missed appointments waste clinic time and delay care for other patients. This project cleans a public appointment dataset, explores what is linked to missed visits, and turns the results into an interactive Power BI dashboard and a presentation with recommendations.

**Questions answered**
1. What is the overall no-show rate?
2. Does wait time matter?
3. Do SMS reminders help?
4. Which age groups miss the most?
5. Does the day of the week matter?

## Key findings

| Finding | Result |
|---|---|
| Overall no-show rate | **20.2%** (about 1 in 5 appointments) |
| Same-day appointments | 4.6% missed |
| Waiting 15+ days | 32.7% missed |
| Age | Teens (13-17) miss the most at 26.6%; seniors (65+) the least at 15.5% |
| Day of the week | Weekdays range only from 19.3% to 21.2%, so little difference |

**Wait time is the strongest driver.** The no-show rate rises steadily from 4.6% (same day) to 22.9% (1-3 days), 25.2% (4-7 days), 30.5% (8-14 days) and 32.7% (15+ days).

**SMS reminders (Simpson's paradox).** Overall, patients who received an SMS missed more visits (27.6% vs 16.7%). This is misleading, because reminders mostly go to appointments booked far ahead, which already have higher no-show rates. Comparing patients with similar waits, SMS is linked to fewer no-shows. At 15+ days it is 37.1% without SMS vs 30.0% with SMS.

## Recommendations

1. **Remind patients early:** send SMS reminders for bookings made 8+ days ahead, where they help most.
2. **Reach younger patients:** target teens and young adults, who miss the most visits.
3. **Cut the waiting time:** offer earlier slots or waitlists, since same-day visits are rarely missed.

## Dataset

- **Source:** [Medical Appointment No Shows](https://www.kaggle.com/datasets/joniarroba/noshowappointments) on Kaggle
- **Size:** 110,527 appointment records from medical centers in Vitória, Brazil, collected over about three months in 2016
- **Grain:** one row = one appointment (a patient can appear more than once)
- **Key columns:** ScheduledDay, AppointmentDay, Age, Gender, Neighbourhood, Scholarship, Hypertension, Diabetes, Alcoholism, Handicap, SMS_received, No-show (`Yes` means the patient **missed** the visit)

The raw data is not included in this repository. Download it from Kaggle and place it in the `data/` folder.

## Tools

- **Excel:** PivotTables for exploring and checking results
- **Power Query:** data cleaning and feature creation
- **Power BI:** data model, DAX measures and the final dashboard
- **PowerPoint:** presentation of findings and recommendations

## Data cleaning (Power Query)

1. Imported the CSV with UTF-8 encoding so neighbourhood names display correctly
2. Renamed misspelled columns (`Hipertension`, `Handcap`, `No-show`)
3. Fixed data types (IDs as text, dates as dates, flags as whole numbers)
4. Removed 5 rows where the appointment date was before the booking date
5. Removed 1 row with an impossible age (-1)
6. Reviewed 7 rows with ages over 100 and kept them as plausible
7. Result: **110,527 rows reduced to 110,521**

**Columns created**

| Column | Purpose |
|---|---|
| `WaitDays` | Days between booking and appointment |
| `WaitGroup` | Same day, 1-3, 4-7, 8-14, 15+ days |
| `Age Group` | Children, Teens, Young Adults, Adults, Middle Age, Seniors |
| `Sms_Status` | "No SMS" or "SMS Received" |
| `ApptWeekDay` | Day name of the appointment |
| `NoShowFlag` | 1 = missed, 0 = attended |

## Power BI measures

```DAX
No-Show Rate = AVERAGE(Appointments[NoShowFlag])
Total Appointments = COUNTROWS(Appointments)
```

Because `NoShowFlag` is 1 or 0, its average is the no-show rate.

## Dashboard

The dashboard shows the overall no-show rate and total appointments, plus four charts:
- No-show rate by wait time
- No-show rate by wait time and SMS status
- No-show rate by age group
- No-show rate by appointment day (Saturday excluded: only 39 appointments)

## Repository structure

```
├── README.md
├── data/                     # place the Kaggle CSV here (not included)
├── Appointment_Noshow_analysis.xlsx
├── Appointments No show dashboards.pbix
├── Appointment_NoShow_Presentation.pptx
└── images/
    └── dashboard.png
```

## How to use

1. Download the dataset from Kaggle and save it in `data/`
2. Open the `.pbix` file in Power BI Desktop
3. If prompted, update the data source path (**Home → Transform data → Data source settings**)
4. Refresh the data

## Limitations

- The data shows an **association, not proof of cause**. SMS reminders were not sent at random.
- One city and one three-month period, so results may not apply elsewhere.
- No information on the reason for a missed visit, the doctor, the appointment type or the distance travelled.
- Saturday has too few appointments to compare.

## Next steps

- Pilot reminders for appointments booked 8+ days ahead and compare missed visits before and after
- Test the findings on the clinic's own data
- Add more patient details to explore other drivers

## Author

**Tolulope Akinrombi**
Data Analyst | Excel, Power Query, Power BI

[LinkedIn](https://www.linkedin.com/in/your-profile) | [GitHub](https://github.com/your-username)
