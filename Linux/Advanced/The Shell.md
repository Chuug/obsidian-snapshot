The `$PATH` variable affects how commands are executed.

###### Create a locale variable
`var=value`
###### Create an environment variable (Uppercase by convention)
`export VAR=value`
`declare -x VAR=value`
`typeset -x VAR=value`
###### Display the variables
`echo $var`
`echo $VAR`
`echo ${VAR}`
###### Display all locale and environment variables
`set`
`declare -p` 
###### Display only environment variables
`env`
`declare -x`
`typeset -x`
`export -p`
###### New terminal (don't import locale variables)
`bash`
###### Set temporary environment variables
`env TZ=EST date`
<small>Change de timezone for date using env</small>
###### Unset variables (Do unset important environment variables)
`unset var`

#### Initialization files
![[Screenshot_2024-04-25_06-43-48.png]]
#### History & cursor shortcuts
![[Screenshot_2024-04-25_06-59-55.png]]
#### History command
![[Screenshot_2024-04-25_07-12-53.png]]