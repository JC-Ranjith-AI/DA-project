# CareLoop: predicting and understanding appointment no-shows

Repository: https://github.com/JC-Ranjith-AI/careloop_no_show_dsai--

## Summary
1. **Part 1 (data):** 1,020 raw rows became 990 clean rows (32 rows were rejected when parsing), and 22.0% of appointments are no-shows.
2. **Part 2 (SQL):** the no-show rate rises with lead time, from 16.2% (1-7 days) to 60.5% (31+ days); the highest clinic with at least 95 appointments is Hadapsar Care Centre at 27.3%.
3. **Part 3 (model):** logistic regression had the best cross-validated ROC-AUC (0.77) and scored 0.79 on the test set; at threshold 0.35 it flags 49 of 198 test appointments, with recall 0.57 and precision 0.57.
4. **Part 4 (GenAI):** of 9 non-blank call notes, 2 were classed forgot_or_no_reminder, and 2 of those 2 belong to appointments with no reminder sent.

## Recommendation
The front desk should phone, the evening before, the appointments the model flags as high risk. Logistic regression was chosen because it had the highest cross-validated ROC-AUC (0.77, and 0.79 on the test set), and a threshold of 0.35 was chosen on training data because it gave the best F1; on the test set it flags 49 of 198 appointments and catches 57% of the real no-shows, in a clinic chain where 22.0% of appointments are no-shows. Lead time is the clearest practical lever: appointments booked 31 or more days ahead are no-shows 60.5% of the time against 16.2% within a week, so those should be confirmed first. The notes point the same way, because 2 of the 2 notes about forgetting belong to appointments with no reminder. In the model, a reminder is associated with lower odds of a no-show (about 0.31 times), but the data are observational, so a small trial should test this before the clinic relies on it. The final threshold should depend on how many follow-up calls the front desk can make in an evening.

## Limitations
1. Only 10 call notes were analysed, so they show examples of real reasons, not clinic-wide patterns.
2. The data are observational, so associations (for example reminders and no-shows) are not proof of cause.
3. The cleaning statistics (medians and outlier fences) were calculated on all rows before the train/test split, so the test set had a small indirect influence on the cleaning.
