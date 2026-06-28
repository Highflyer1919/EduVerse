# Data

This project uses the **ASSISTments 2012–2013** dataset for evaluation.

## Download

1. Visit the ASSISTments data page:  
   https://sites.google.com/site/assistmentsdata/home/2012-13-school-data-with-affect

2. Download the dataset and place the CSV file(s) in this `data/` directory.

## Format Expected

The evaluation scripts expect a CSV with at minimum the following columns:

| Column | Description |
|--------|-------------|
| `student_id` | Unique student identifier |
| `skill_name` | Skill/topic being assessed |
| `correct` | 1 if correct, 0 if incorrect |
| `attempt_count` | Attempt number for this problem |

## Citation

Heffernan, N. T., & Heffernan, C. L. (2014). The ASSISTments ecosystem: Building 
a platform that brings scientists and teachers together for minimally invasive 
research on human learning and teaching. Journal of Educational Data Mining, 6(1), 1–26.
