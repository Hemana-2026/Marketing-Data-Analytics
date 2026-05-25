CREATE DATABASE MARKETING_DB;
 
 CREATE TABLE Marketing_Data (
    Advertiser VARCHAR(50),
    Campaign VARCHAR(100),
    Partner VARCHAR(50),
    Channel VARCHAR(50),
    Country VARCHAR(50),
    Region VARCHAR(50),
    Impressions BIGINT,
    Spend INT,
    Clicks BIGINT,
    Conversions BIGINT,
    Month INT,
    Year INT
);

DROP TABLE MARKETING_DATA

TRUNCATE TABLE CAMPAIGN_DATA;


SELECT * from MARKETING_DATA;

SELECT TOP 10* FROM MARKETING_DATA BY CONVERSIONS;

Update Marketing_data set country = 'USA' where country ='N/A';

Update Marketing_data set Impressions = 'NULL' where Impressions ='0';


Select Campaign,count(*) from marketing_data group by campaign Having count(*)>1;

Select Campaign, (Clicks*100.0/Impressions) as CTR from marketing_data;

--top 10 list of campaigns which has more conversions

SELECT TOP 10
    advertiser,
    SUM(conversions) AS total_conversions
FROM marketing_data
GROUP BY advertiser
ORDER BY total_conversions DESC;

--> CTR

Select campaign,clicks,impressions,(clicks*100.0)/NULLIF(impressions,0) as CTR from marketing_data;

-->CPC

Select advertiser,campaign,(spend/NULLIF(Clicks,0)) As CPC from marketing_data;

-->CPM

Select campaign,(spend*1000.0/NULLIF(Impressions,0)) as  CPM from marketing_data;

-->Conversion Rate CVR

Select Advertiser, campaign,(Conversions*100.0/nullif(clicks,0)) as Conversion_Rate from marketing_data;

-->CPA

Select campaign,(Spend/NULLIF(Conversions,0)) As CPA from marketing_data;

-->Analysis queries

Select top 10 
campaign,
Sum(Spend) as Total_spend,
Sum(Conversions) as Total_Conversions
from marketing_data
Group By campaign
Order by total_conversions Desc;

-->Channel Anaysis

SELECT 
Channel,
SUM(Clicks) AS Total_Clicks,
SUM(Conversions) AS Total_Conversions,
(SUM(Clicks) * 100.0 /NULLIF(SUM(Impressions),0)
) AS AVG_CTR
FROM marketing_data
GROUP BY Channel
ORDER BY Total_Conversions DESC;

-->USING CTES

WITH CTRDATA As
(
SELECT campaign , (
        SUM(Clicks) * 100.0 /
        NULLIF(SUM(Impressions),0)
        ) AS AVG_CTR from marketing_data
group by campaign
)
Select top 10 * from CTRDATA order by AVG_CTR DESC;

-->Region Analysis

Select Region,
Sum(spend) as total_spend,
Sum(Conversions) as Total_Conversions,
Sum((Conversions*100.0)/(nullif(clicks,0))) as CVR from marketing_data
group by region 
order by CVR DESC;


--> Monthly analysis

Select Month,
Sum(spend) as total_spend,
Sum(Conversions) as Total_Conversions,
Sum((Conversions*100.0)/(nullif(clicks,0))) as CVR from marketing_data
group by month
order by total_conversions desc;

-->Year Analysis

Select Year,
Sum(spend) as total_spend,
Sum(Conversions) as Total_Conversions from marketing_data
group by Year
order by total_conversions desc;

--> Campaign ranking

Select campaign, Sum(Conversions) as total_conversions,

DENSE_RANK() OVER(ORDER BY SUM(Conversions) Desc) as Campaign_Rank from marketing_data
Group by Campaign;


Select campaign, Sum(Conversions) as total_conversions,

RANK() OVER(ORDER BY SUM(Conversions) Desc) as Campaign_Rank from marketing_data
Group by Campaign;


