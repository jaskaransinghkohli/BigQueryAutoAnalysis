# BigQueryAutoAnalysis

Auto-Claim Genius: An AI-Powered Insurance Claims Resolution Engine¶
Problem Statement

The auto insurance industry is inundated with a complex mix of structured and unstructured data, from policy details and vehicle specifications to raw accident images and textual descriptions. The traditional process of manually reviewing this data to assess damage, determine fault, and estimate repair costs is notoriously slow, inconsistent, and resource-intensive. This inefficiency creates a significant operational bottleneck, leading to delayed resolutions, increased costs, and a frustrating customer experience. The core problem is not a lack of data, but the inability to unlock and synthesize insights from these varied formats at scale.
Impact Statement

Auto-Claim Genius directly addresses this challenge by creating an end-to-end, AI-driven workflow within Google BigQuery. Our solution transforms the claims process from a manual, multi-day task into a streamlined, automated analysis. The material impact is a significant reduction in claim cycle times, a decrease in operational overhead, and a dramatic improvement in the consistency and accuracy of initial damage assessments. This allows claims adjusters to focus their expertise on the most complex cases, leading to faster payouts, reduced fraud, and ultimately, higher customer satisfaction and retention.
Solution Overview & Technical Walkthrough

Auto-Claim Genius is a working prototype that demonstrates how BigQuery's native AI capabilities can build an intelligent business application directly on top of a data warehouse. Our solution follows a three-phase process to turn raw, mixed-format data into polished, actionable business intelligence.
Phase 1: Data Preparation & Multimodal Enrichment

This phase addresses the challenge of combining disparate data sources into a single, analysis-ready master table. We heavily utilized BigFrames, the Python API for BigQuery, to perform large-scale data manipulation without moving data out of the warehouse.

    Unified Data Loading: We began by loading our structured tables (Claims, Policies, Vehicles, Policyholders) and unstructured data sources (claim_images_raw, Image_Claims_Analysis) into BigFrames DataFrames.
    Multimodal Fusion: We then performed a series of joins to create a master feature table. This process seamlessly combines structured data (e.g., accident_type, vehicle_age) with unstructured data (the text description of accident images), a core tenet of the Multimodal Pioneer approach.
    Feature Engineering: We derived new, valuable features directly in BigQuery, such as days_to_file_claim and composite_risk_score, to provide richer context for our AI models.

Phase 2: AI-Powered Prediction of Key Claim Attributes

This is the heart of our solution and a direct implementation of the AI Architect approach. The goal was to use generative AI to intelligently predict and fill in the five critical, but often missing, columns in our claims dataset.

    Contextual Prompting: For each claim, we dynamically constructed a rich, contextual prompt. This prompt provided the Gemini model with a comprehensive summary of the incident, including structured data points and the unstructured text analysis of the accident image.
    Generative Prediction with Gemini: We used bigframes.ml.llm.GeminiTextGenerator to analyze this prompt and generate predictions for the five target fields:
        airbag_deployed (True/False)
        drivable_post_accident (True/False)
        predicted_damage_severity (Low, Medium, or High)
        predicted_quote (A specific USD amount)
        damage_location (e.g., Front Bumper)
    Robust Parsing: To ensure data quality, we applied robust parsing techniques using regular expressions (regex) to extract the structured information from the model's text output and populate our final DataFrame.
