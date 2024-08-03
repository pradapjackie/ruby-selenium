# Running Selenium Tests with GitHub Actions

This document provides instructions on how to run Selenium tests using GitHub Actions. The tests are written in Ruby and use the Selenium WebDriver for browser automation.

## Prerequisites

Before setting up GitHub Actions, ensure you have:

- A Ruby script with Selenium tests.
- A `Gemfile` including necessary gems (`selenium-webdriver` and optionally `webdrivers`).
- Your repository hosted on GitHub.

## Setting Up GitHub Actions

1. **Create a Workflow File**

   Create a GitHub Actions workflow file to automate running your Selenium tests. 

   - Create a directory named `.github/workflows` in the root of your repository if it doesn’t already exist.
   - Add a YAML file (e.g., `selenium-tests.yml`) to this directory with the following content:

     ```yaml
     name: Selenium Test

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest

    services:
      selenium:
        image: selenium/standalone-chrome:latest
        ports:
          - 4444:4444

    steps:
    - name: Checkout repository
      uses: actions/checkout@v2

    - name: Set up Ruby
      uses: ruby/setup-ruby@v1
      with:
        ruby-version: 3.0

    - name: Install dependencies
      run: |
        gem install bundler
        bundle install

    - name: Run Selenium test
      run: |
        sudo apt-get update
        sudo apt-get install -y chromium-chromedriver
        export PATH="$PATH:/usr/lib/chromium-browser/"
        ruby test.rb # Replace with the path to your test script
     ```

   Replace `path/to/your_script.rb` with the relative path to your Selenium test script.

2. **Commit and Push Changes**

   After creating the workflow file, commit and push the changes to your GitHub repository:

   ```bash
   git add .github/workflows/selenium-tests.yml
   git commit -m "Add GitHub Actions workflow for Selenium tests"
   git push
