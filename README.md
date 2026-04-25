# KAYAK Project

Find the best holiday destinations based on weather forecast and the best hotels in those cities.

## Requirements

- Python 3.11+
- Anaconda or Miniconda installed
- AWS account with S3 access
- OpenWeatherMap API key
- Google Chrome browser (for web scraping)

## Quick Start

### 1. Clone the repository
```bash
git clone <your-repo-url>
cd kayak_project
```

### 2. Create the Conda environment
```bash
conda env create -f environment.yaml
```

### 3. Activate the environment
```bash
conda activate kayak_project
```

### 4. Configure credentials

Create a `.env` file in the project root:
```bash
echo "API_KEY=your_openweathermap_api_key" > .env
```

### 5. Configure AWS credentials

Option A: Using AWS CLI
```bash
aws configure
```

Option B: Using environment variables
```bash
export AWS_ACCESS_KEY_ID=your_key
export AWS_SECRET_ACCESS_KEY=your_secret
export AWS_DEFAULT_REGION=eu-west-1
```

### 6. Create S3 bucket
```bash
aws s3 mb s3://kayak_project_bucket
```

### 7. Run the project
```bash
python kayak_project_complete.py
```

## Project Structure

```
kayak_project/
├── environment.yaml              # Conda environment file
├── kayak_project.py              # Main script
├── .env                          # API credentials
├── .gitignore                    # Git ignore file
└── README.md                     # This file
```

## Getting API Keys

### OpenWeatherMap API
1. Go to https://openweathermap.org/api
2. Sign up for free account
3. Go to API keys section
4. Copy your API key
5. Add to `.env` file: `API_KEY=your_key`

### AWS Credentials
1. Go to AWS Console
2. Create IAM user with S3 permissions
3. Generate access keys
4. Use `aws configure` to set them up

## Project Flow

```
1. Get coordinates for 35 French cities (Nominatim API)
2. Fetch 7-day weather forecast (OpenWeatherMap API)
3. Score cities based on weather (custom algorithm)
4. Identify top-5 destinations
5. Scrape top-20 hotels in each city (Selenium/Booking.com)
6. Create interactive maps (Plotly)
7. Upload results to S3 (boto3)
```

## Output Files (uploaded to S3)

After running the script, the following files are uploaded to `kayak_project_bucket`:

- **cities_coordinates.csv** - Coordinates of all 35 cities
- **weather_scores.csv** - Top-5 destinations with weather scores
- **hotels.csv** - Top-20 hotels with ratings and URLs
- **map_destinations.html** - Interactive map of best destinations
- **map_hotels.html** - Interactive map of best hotels

## Troubleshooting

### "ChromeDriver not found"
```bash
# webdriver-manager will auto-download ChromeDriver
# Make sure Chrome/Chromium is installed on your system
```

### "API_KEY not found"
```bash
# Check that .env file exists in project root
# Make sure API_KEY is set correctly
```

### "S3 upload fails"
```bash
# Check AWS credentials: aws sts get-caller-identity
# Check bucket exists: aws s3 ls
# Check bucket permissions: aws s3api get-bucket-acl --bucket kayak_project_bucket
```

### "Booking.com blocks scraping"
```bash
# The script includes anti-blocking measures
# If still blocked, wait a few minutes and try again
# Consider using a VPN or proxy if needed
```

## Understanding the Weather Score

The weather score is calculated as:

```
Weather_Score = (0.4 × temp_score) + (0.2 × humidity_score) + (0.3 × rain_score) + (0.1 × stability_score)
```

Where:
- **temp_score**: How close to ideal temperature (25°C)
- **humidity_score**: How dry the air is (0% = 1.0, 100% = 0.0)
- **rain_score**: Probability of no rain
- **stability_score**: How consistent temperatures are (lower variance = higher score)

Final score = mean daily score - standard deviation (penalizes unpredictable weather)

## Documentation

- **Nominatim API**: https://nominatim.org/
- **OpenWeatherMap API**: https://openweathermap.org/api
- **Selenium**: https://www.selenium.dev/documentation/
- **Plotly**: https://plotly.com/python/
- **Boto3**: https://boto3.amazonaws.com/v1/documentation/api/latest/index.html

## License

This project is for educational purposes.

## Author

Mounia Tonazzini
