# nJira

[![CRAN_Status_Badge](http://www.r-pkg.org/badges/version/nJira?color=brightgreen)](https://cran.r-project.org/package=nJira)
[![CRAN downloads](http://cranlogs.r-pkg.org/badges/grand-total/nJira?color=brightgreen)](http://cranlogs.r-pkg.org/badges/grand-total/nJira)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)

SQL like query interface for [Jira](https://www.atlassian.com/software/jira) in R language

This package provides a SQL like query interface to fetch data from any [Jira](https://www.atlassian.com/software/jira) installation. The data is fetched into R dataframe (using [Jira REST API](https://developer.atlassian.com/cloud/jira/platform/rest/v2/))

Key features:
  * Data is fetched using [Jira REST API](https://developer.atlassian.com/cloud/jira/platform/rest/v2/) therefore can be used over web without needing to connect to any database.
  * Function to authenticate into Jira instance using `jira.login()`
  * Function to fetch METADATA from Jira instance using `jira.metadata()`
  * Function to QUERY jira data with SQL like syntax using `jira.query()`
  
  
## Installation

CRAN version:
```r
install.packages("nJira")
```

Development version:
```R
# install.packages("devtools")
library(devtools)
devtools::install_github("datacosmos-br/nJira")
```

## Getting Started
You should have an account in the Jira instance to query its data. You can use the same log-in credentials that you use on your Jira website.

Assuming your Jira credentials are as follows

  * Jira Environment (web link): https://issues.apache.org/jira
  * Jira Username: jiraTestUser
  * Jira Password: jiraTestPwd

### Authenticate in Jira using the following command:
```{r}
jira.login(jira.env = "https://issues.apache.org/jira", jira.user = "jiraTestUser", jira.pwd = "jiraTestPwd")
```
### Fetch the Metadata of Jira using the following command:
```{r}
jiraMetadata <- jira.metadata()
```

### Query Jira tables and fields (lookup via metadata) using the following command:
```{r}

issues <- jira.query(table = "issues", fields = "id AS IssueId, Created, Status, Priority", 
where = "project = 'HIVE' AND created >= '2019-01-01' AND created <= '2019-12-31' AND 
Status IN ('Open', 'Closed', 'Resolved')")

issues <- jira.query(table = "issues", fields = "id AS IssueId, Created", 
where = "'cf[10021]' = 'ABCD' AND Created > '2019-01-01'")


history <- jira.query(table = "history", fields = "id AS IssueId, toString AS Status, 
COUNT(fromString) AS Count", where = "id = 'HIVE-22692' AND field = 'status'", 
groupby = "id,toString")
```

## Code of Conduct
Please note that the 'nJira' project is released with a
[Contributor Code of Conduct](CODE_OF_CONDUCT.md).
By contributing to this project, you agree to abide by its terms.
