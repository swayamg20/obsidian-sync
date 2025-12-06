
[[2024-11-11]]
- [ ] AI sdk demo ready
	- [ ] triggering complete
	- [ ] UI fixes
- [ ] ss2code 
- [ ] voice agent dashboard (download csv)


[[2024-11-19]]
- [ ] ss2code fix tailwind/css (fonts) render
- [ ] voice agent dashboard add
- [ ] AI sdk



we have a table:

need to find revenue till nov'24 (for FY25)

how much revenue we have to distribute
we have FY24 revenue
I subtracted total fee paid to FY24
%% this is total revenue to be considered in the calculation %%
DOUBTS:
Question: regarding the unpaid fee, when do I need to consider it.
Question: consider isDropped column also.

duration will be calculated from--
startdate--> batch start date or enrolment date (whichever is later)
enddata--> batch end date
subtract both

get the total months for revenue calculation


now: fee per month is --> total revenue / duration (months) 

required_interval_start_date:
if batch start is after 1st April then batch strart
else 1at April

now: for what duration we need revenue of (till Nov'24)
	need to calculate end date (required_interval_end_date):
		if batch end date > 30th Nov then take 30th Nov
		else take the batch end date

total required months of revenue till Nov'24
required_interval_end_date - required_interval_start_date

now finally multiplied it with fee per month
