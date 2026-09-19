# Purchase Intent Classifier

## About
* View the application and performance analysis in **main.ipynb**.
* This project was created for a course on LLMs.

## Background
An enterprise B2B technology company may receive hundreds and sometimes thousands of inbound leads per day. These leads can come through many ways, and this application assumes that lead intake will be through a contact sales form on the website. In order to prioritize these leads effectively and route them to the correct sales team, the triage team needs a more accurate indicator of purchase intent rather than traditional methods of evaluating purchase intent, namely lead scoring, which is usually based on behavioral and demographic scores and doesn’t do as good of a job at ensuring that sales does not waste its time on low-intent leads or miss potential revenue due to high-intent leads who may not be as interested in consuming marketing content, but is willing to talk to sales.

## Task
This application classifies inbound B2B sales leads across three dimensions using GPT-4o:

`intent_level` — `high` / `medium` / `low` / `na`  
`opportunity_size` — `large` / `medium` / `small` / `na`  
`purchase_timeline` — `immediate` / `3-6 months` / `6+ months` / `na`  
Each prediction is evaluated against ground truth labels using accuracy, precision, recall and confusion matrices.  

## Comparison to Traditional Methods - Lead Scoring
For those who are not familiar with what lead scoring is, it is a methodology that ranks each prospect's total potential and readiness to become a customer. Lead scoring ranks by two categories: a lead's demographic traits (e.g., job title/seniority, company size, industry, job function, sales region), and a lead's behavioral traits, i.e., the actions they took that show interest in the product online (e.g., registering for a webinar, opening and clicking an email X number of times, signing up for a free trial, subscribing, etc.). The higher the demographic score, the more aligned this person is with the demographic traits of the target audience. The higher the behavioral score, the more interest that this lead shows in the product.  

Every company's lead scoring model varies. Generally, it is a widely used method of evaluating a lead's likelihood to become a customer but is actually somewhat weak in predicting a lead's readiness to purchase. This results in sales wasting its time on low-intent leads marked as hot, or missing potential revenue due to overlooking leads that are ready to buy but don't consume any marketing content and thereby are classified as cold. Leads usually have a numeric score and a level associated with this number (hot/warm/cool/cold); for example, a hot lead would have a lead score from 76-100, a warm lead's score would be from 51-75 and so on. The way most enterprise B2B technology companies would categorize a lead who fills out a form online to be contacted by sales is hot, just by default: As in, if you request to be contacted by sales, you are automatically considered a hot lead.  

Sales knows that just because a lead requested to be contacted, that the probability of converting to an actual opportunity is not necessarily aligned with high intent_level, the right opportunity_size and appropriate purchase_timeline. Every lead is different. When a lead requests to be contacted by sales, the lead typically includes a message in the request with some context for the request. This application evaluates each lead on these three categories because it knows that there are more dimensions to a lead's probability of purchasing than just a lead being considered hot. To do so, it evaluates the semantic content of the lead's message, in addition to demographic traits and behavioral traits. All of these criteria are included in the synthetic dataset created by Claude and are evaluated by GPT-4o in order to classify across the three dimensions stated. So, in line with traditional lead scoring methods, we have a column lead_score_level where all leads are marked as hot; this LLM application will then predict intent_level, opportunity_size and purchase_timeline in addition to lead_score_level (as in, it will not replace the original classification, but will continue to enrich the lead through new criteria).  

For the purposes of this experiment, we will be considering the label intent_level of high as essentially equivalent to a lead_score_level of hot in definition. No matter what the level of intent_level, all leads will get a response from sales; but for the purposes of recording actual purchase intent correctly, we will add the intent_level column, in addition to the opportunity_size and purchase_timeline, using this LLM application.  

## Assumptions
* The name of my hypothetical company is PipelineIQ - we sell a CRM solution.
* I will model a B2B enterprise technology company’s products, target audience, sales organization and sales process in order to create the dataset for this application. Please note, these criteria are loosely applied throughout this notebook but will help set the stage for the application in general.
* This application is intended to support the lead triage team whose responsibility is to route leads to the right sales team and surface leads that indicate high urgency or high purchase intent.
* The dataset is built as if there is an API integration between a CRM and the LLM application.
