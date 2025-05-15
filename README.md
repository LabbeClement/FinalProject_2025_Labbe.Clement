# Video Recommendation System

This repository contains a video recommendation system built using content-based filtering methods. The project uses a dataset of user interactions with short-form videos to develop personalized content recommendations.

## Table of Contents
1. [Overview](#overview)
2. [Dataset](#dataset)
3. [Methodology](#methodology)
4. [Feature Engineering](#feature-engineering)
5. [Model Development](#model-development)
6. [Evaluation](#evaluation)
7. [Results](#results)
8. [Conclusions](#conclusions)

## Overview

This project implements a content-based video recommendation system that analyzes user behavior and video characteristics to provide personalized recommendations. The system focuses on recommending videos that match users' preferences based on their past viewing behavior.

## Dataset

The dataset used in this project is the KuaiRec 2.0, which contains:

- User-video interactions (watch time, engagement metrics)
- Video metadata (categories, duration, etc.)
- Engagement metrics (likes, comments, shares)
- Temporal data (timestamps of interactions)

Key dataset statistics:
- 4,269,399 user-video interactions
- 1,411 unique users
- 3,327 unique videos
- 31 different content categories

The dataset provides rich engagement metrics including:
- Watch ratio (play_duration / video_duration)
- Total likes, comments, and shares
- Average play progress
- User category preferences

## Methodology

The project follows a systematic approach to building a recommendation system:

1. **Data Loading and Preprocessing**:
   - Loading multiple dataset files
   - Handling missing values and inconsistencies
   - Filtering out extreme values in watch ratios (below 5th and above 99.99th percentile)
   - Converting categorical data into appropriate numeric representations

2. **Exploratory Data Analysis**:
   - Understanding user behavior patterns
   - Analyzing video popularity metrics
   - Identifying category preferences

3. **Feature Engineering**:
   - Creating user profiles based on category preferences and engagement patterns
   - Developing video features based on popularity and engagement metrics
   - Generating interaction features between users and videos

4. **Model Development**:
   - Building a content-based recommendation system using similarity-based techniques
   - Implementing a recommendation algorithm that can generate personalized video suggestions

5. **Evaluation**:
   - Training-test split (80/20) for evaluation
   - Implementing evaluation metrics: Precision@k, Recall@k, and Mean Average Precision (MAP)
   - Testing with different values of k to understand performance across recommendation list sizes

## Feature Engineering

### User Features
- **Engagement statistics**: video_count, avg_watch_ratio, total_play_time, complete_views, avg_play_progress
- **Category preferences**: Distribution of watched videos across 31 content categories
- **Temporal patterns**: morning_ratio, afternoon_ratio, evening_ratio, night_ratio, weekend_ratio

### Video Features
- **Popularity metrics**: view_count, like_engagement, comment_engagement, share_engagement, daily_popularity
- **Engagement quality**: avg_watch_ratio, complete_view_ratio
- **Normalized metrics**: All popularity features were normalized using MinMaxScaler to prevent dominance of certain metrics

### Interaction Features
- **User-Video affinity**: Ratio of a user's watch ratio for a video to their average watch ratio
- **Category-based matching**: Alignment between user category preferences and video categories

## Model Development

The implementation uses a content-based filtering approach that:

1. **Creates a video similarity matrix** using cosine similarity between video feature vectors
2. **Identifies user preferences** by analyzing videos with high watch ratios (>0.7)
3. **Generates recommendations** by finding videos similar to those the user has previously enjoyed
4. **Filters recommendations** by removing videos the user has already watched

The content-based recommender class has these key components:
- Video feature representation
- Video similarity computation
- User preference extraction
- Recommendation generation logic

## Evaluation

The evaluation framework uses:

- **Training-Test Split**: 80% of interactions for training, 20% for testing
- **Relevant Items Definition**: Videos with watch ratio > 0.7 in the test set are considered relevant
- **Metrics**:
  - Precision@k: Proportion of recommended items that are relevant
  - Recall@k: Proportion of relevant items that are recommended
  - Mean Average Precision (MAP): Average precision across different recall levels

Tests were conducted with varying values of k (5, 7, 10, 12, 15, 17, 20) to understand how performance changes with different recommendation list sizes.

## Results

The results show consistent performance across different values of k:

| k  | Precision@k | Recall@k | MAP     |
|----|-------------|----------|---------|
| 5  | 0.6080      | 0.0086   | 0.0070  |
| 7  | 0.6143      | 0.0123   | 0.0095  |
| 10 | 0.6020      | 0.0172   | 0.0129  |
| 12 | 0.6025      | 0.0207   | 0.0152  |
| 15 | 0.6067      | 0.0260   | 0.0186  |
| 17 | 0.6135      | 0.0298   | 0.0211  |
| 20 | 0.6050      | 0.0346   | 0.0243  |

Key observations:
- **Precision remains stable** (around 60-61%) across different k values
- **Recall increases** with larger k values, as expected
- **MAP (Mean Average Precision)** also increases with k, showing that larger recommendation lists generally provide better coverage of relevant items

The model demonstrates good precision, indicating that most recommended videos are relevant to the user's interests. The increasing recall with larger k values shows that we can capture more of the user's preferred videos by increasing the recommendation list size.

## Conclusions

The content-based recommendation system developed in this project demonstrates several strengths:

1. **Consistent Precision**: The system maintains high precision (~60%) across different recommendation list sizes, indicating it reliably recommends relevant content.

2. **Personalization Capability**: By leveraging user viewing history and content features, the system can provide personalized recommendations without requiring collaborative data.

3. **Scalability**: The approach can handle new videos without suffering from the cold-start problem often seen in collaborative filtering methods.

Potential improvements for future work:

1. **Hybrid Approach**: Combining content-based filtering with collaborative filtering could improve recommendation quality.

2. **Temporal Dynamics**: Incorporating temporal patterns in user behavior could enhance the model's ability to adapt to changing preferences.

3. **Deep Learning Methods**: Using deep neural networks for feature extraction and similarity computation could capture more complex patterns in the data.

4. **Diversity Enhancement**: Adding mechanisms to ensure diversity in recommendations while maintaining relevance.

Overall, this content-based recommendation system provides a solid foundation for personalized video recommendations that can be further enhanced with additional techniques and features.
