# Forecasting Monthly Player Activity for Live Service Games

## Adilet Shabay

## Project Summary

This repository contains an end-to-end forecasting project that predicts the next 12 months of monthly average player activity for a set of live service and multiplayer games. The dataset is built by combining monthly player counts from SteamCharts with two simple external signals: a Steam News API based proxy for update cadence and a Google Trends based proxy for public attention. The modeling workflow converts each game time series into supervised windows and trains a small PyTorch regression model to produce 12-step forecasts. The project also includes baseline comparisons and evaluation metrics such as MAE, RMSE, and sMAPE, plus a precision summary showing how often forecasts fall within relative error thresholds.

## Repository Structure

* Final_Project_EDA.ipynb

This notebook extracts and assembles the dataset. It scrapes monthly player history from SteamCharts, pulls Steam News posts and aggregates a monthly update-intensity signal via keyword matching, fetches Google Trends monthly interest scores, merges all sources into a continuous monthly panel per game, creates forecasting features including calendar seasonality and shifted rolling summaries, runs basic consistency checks, and exports the final processed dataset used by the modeling notebook.

* Final_Project_Analysis.ipynb
This notebook loads the processed dataset exported by the EDA notebook and performs the forecasting workflow. It creates supervised training windows with a 12-month forecast horizon, performs time-based train, validation, and test splits, evaluates simple baseline forecasts, trains a small PyTorch MLP with early stopping, computes evaluation metrics, produces precision tables, and plots forecasted versus actual values for example games. It also saves the trained model and preprocessing statistics as reusable artifacts.

data/
raw/ contains cached SteamCharts HTML pages
external/ contains cached Steam News API JSON and Google Trends CSV files
processed/ contains the exported dataset monthly_panel_with_exog.csv used by the modeling notebook

artifacts/
Contains saved model checkpoints and normalization statistics produced during training

## How to Reproduce
Run Final_Project_EDA.ipynb first. This creates data/processed/monthly_panel_with_exog.csv. Then run Final_Project_Analysis.ipynb, which reads the processed dataset and trains and evaluates the forecasting model without requiring any additional scraping. If external data calls fail due to rate limits, cached files in data/external and data/raw allow re-runs to remain stable once the data has been collected.

## Outputs
The EDA notebook produces a cleaned and feature-rich monthly panel dataset and basic data quality summaries. The modeling notebook produces baseline results, trained model performance metrics, a precision summary table, forecast-versus-actual plots over the 12-month horizon, and a saved model checkpoint for reproducibility.
