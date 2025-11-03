# Tiki product API Crawling

## Table of Content
- [Tiki product API Crawling](#tiki-product-api-crawling)
  - [Table of Content](#table-of-content)
  - [❓ Problems](#-problems)
  - [📋 Preparation](#-preparation)
  - [📝 Tasks](#-tasks)
  - [⚙️ Folders structure](#️-folders-structure)
  - [📝 How to set up and run](#-how-to-set-up-and-run)
  - [Result](#result)


## ❓ Problems
- Based on the given product id list, crawling information of 200.000 Tiki products and save them as .json files
- Each file has information of about 1000 products (as a batch).
- The information to be retrieved includes: id, name, url_key, price, description, images url.
- Require standardizing the content in "description" and finding a way to shorten the time to retrieve data.


## 📋 Preparation
- Download list of product ids and configure the Tiki API url

## 📝 Tasks
- Based on the list of product ids and API product data, crawl products' information
- Processing crawled data, descriptions of products particularly
  - Replace `<p>` và `</p>` with `\n`
  - Replace `<br />` with `\n`
  - Replace `<li>` with "- ", `</li>` with `\n`
  - Remove all remaining html tag
  - Remove unnecessary blank lines
- Load crawled data into database

**Select appropriate approach for crawling**
- Option 1: Sequencing: the lowest approach
- Option 2: Multiprocessing: faster than sequencing and consistent
  => In this project, its better to use Multiprocessing (Pool) with the package fetch_data in tiki_fetcher


## ⚙️ Folders structure
- **data folder** contains raw data from API including the data of each products and the product id which cannot fetch due to 404 error or timeout error or other http errors
- **output folder** contains all product data after normalizing the description including: finding all image urls in the description, replacing ```<br />``` with "\n", replacing ```<p>``` and ```</p>``` with "\n", replacing ```<li>``` with "- " and ```</li>``` with "\n", removing all remaining html tag


## 📝 How to set up and run
- Create virtual environment and install all necessary libraries in `requirements.txt`
- Execute package `fetch_data` in `data_fetcher`
- Execute `process_description.py` to normalize the html tags and extract image sources in the description and add them to the images field


## ✅ Result
- 198942 products are collected  <br />
- 1058 product ids with 404 Not Found error <br />
- 0 product ids with HTTP error (except for 404) <br />
- 0 product ids with timeout error
