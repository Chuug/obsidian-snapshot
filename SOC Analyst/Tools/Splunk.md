**Start Splunk service**
```shell
sudo service splunkd start
```

#### Custom filters
**Exclude entries beginning with a specific character**
<small>The ScriptBlockText field will never begin with {</small>
```text
index="boogeyman1" | where NOT match(ScriptBlockText, "^{")
```

#### Filters

**Fields**
<small>+ to add fields, - to remove fields</small>
```text
index=logs | fields + host - User
```

**Search**
<small>Search specific word in logs</small>
```text
index=logs | search "powershell"
```

**Dedup**
<small>Remove duplicate</small>
```text
index=logs | table EventID User Image Hostname | dedup EventID
```

**Rename**
<small>Rename a field</small>
```text
index=logs | rename User as Employees
```

#### Structure

**Table**
<small>Create a custom table with selected fields</small>
```text
index=logs | table EventID Hostname SourceName
```

**Head & tail**
<small>Show the x top/bottom results</small>
```text
index=logs | tail/head 10
```

**Sort**
<small>Sort the result</small>
```text
index=logs | table EventID User | sort EventID
```

**Reverse**
<small>Reverse the sorted result</small>
```text
index=logs | table EventID User | sort EventID | reverse
```

#### Transformational
Used for graphs

**Top**
<small>Shows the top x of the field</small>
```text
index=logs | top limit=3 EventID
```

**Rare**
<small>The opposite of Top</small>
```text
index=logs | rare limit=3 EventID
```

**Highlight**
<small>Highlight words in the raw format</small>
```text
index=logs | highlight User Password
```


#### Stats
| **Command**       | **Explanation**                                                   | **Example**              |
| ----------------- | ----------------------------------------------------------------- | ------------------------ |
| **Average  <br>** | This command is used to calculate the average of the given field. | stats avg(product_price) |
| **Max  <br>**     | It will return the maximum value from the specific field.         | stats max(user_age)      |
| **Min**           | It will return the minimum value from the specific field.         | stats min(product_price) |
| **Sum**           | It will return the sum of the fields in a specific value.         | stats sum(product_cost)  |
| **Count**         | The count command returns the number of data occurrences.         | stats count(source_IP)   |

#### Regex
**Isolate a value from string in a new column**
```text
rex field=form_data "passwd=(?<creds>\w+)" | table creds
```
![[Screenshot_2024-07-10_06-43-43.png]]
The column is named **creds** by `(?<creds>...)`, it search the `passwd=` beginning and add all numerical, alphabetical and underscores with the `\w+`