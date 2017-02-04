  
  #Networking
  1. USERNAME [a-zA-Z0-9._-]+
  2. USER %{USERNAME}
  3. EMAILLOCALPART [a-zA-Z][a-zA-Z0-9_.+-=:]+
  4. EMAILADDRESS %{EMAILLOCALPART}@%{HOSTNAME}
  5. INT (?:[+-]?(?:[0-9]+))
  6. BASE10NUM (?<![0-9.+-])(?>[+-]?(?:(?:[0-9]+(?:\.[0-9]+)?)|(?:\.[0-9]+)))
  7. NUMBER (?:%{BASE10NUM})
  8. BASE16NUM (?<![0-9A-Fa-f])(?:[+-]?(?:0x)?(?:[0-9A-Fa-f]+))
  9. BASE16FLOAT \b(?<![0-9A-Fa-f.])(?:[+-]?(?:0x)?(?:(?:[0-9A-Fa-f]+(?:\.[0-9A-Fa-f]*)?)|(?:\.[0-9A-Fa-f]+)))\b
  11. POSINT \b(?:[1-9][0-9]*)\b
  12. NONNEGINT \b(?:[0-9]+)\b
  13. WORD \b\w+\b
  14. NOTSPACE \S+
  15. SPACE \s*
  16. DATA .*?
  17. GREEDYDATA .*
  18. QUOTEDSTRING (?>(?<!\\)(?>"(?>\\.|[^\\"]+)+"|""|(?>'(?>\\.|[^\\']+)+')|''|(?>`(?>\\.|[^\\`]+)+`)|``))
  19. UUID [A-Fa-f0-9]{8}-(?:[A-Fa-f0-9]{4}-){3}[A-Fa-f0-9]{12}
  20. # URN, allowing use of RFC 2141 section 2.3 reserved characters
  21. URN urn:[0-9A-Za-z][0-9A-Za-z-]{0,31}:(?:%[0-9a-fA-F]{2}|[0-9A-Za-z()+,.:=@;$_!*'/?#-])+
  
  #Networking
  24. MAC (?:%{CISCOMAC}|%{WINDOWSMAC}|%{COMMONMAC})
  25. CISCOMAC (?:(?:[A-Fa-f0-9]{4}\.){2}[A-Fa-f0-9]{4})
  26. WINDOWSMAC (?:(?:[A-Fa-f0-9]{2}-){5}[A-Fa-f0-9]{2})
  27. COMMONMAC (?:(?:[A-Fa-f0-9]{2}:){5}[A-Fa-f0-9]{2})
  28. IPV6 ((([0-9A-Fa-f]{1,4}:){7}([0-9A-Fa-f]{1,4}|:))|(([0-9A-Fa-f]{1,4}:){6}(:[0-9A-Fa-f]{1,4}|((25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)(\.(25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)){3})|:))|(([0-9A-Fa-f]{1,4}:){5}(((:[0-9A-Fa-f]{1,4}){1,2})|:((25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)(\.(25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)){3})|:))|(([0-9A-Fa-f]{1,4}:){4}(((:[0-9A-Fa-f]{1,4}){1,3})|((:[0-9A-Fa-f]{1,4})?:((25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)(\.(25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)){3}))|:))|(([0-9A-Fa-f]{1,4}:){3}(((:[0-9A-Fa-f]{1,4}){1,4})|((:[0-9A-Fa-f]{1,4}){0,2}:((25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)(\.(25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)){3}))|:))|(([0-9A-Fa-f]{1,4}:){2}(((:[0-9A-Fa-f]{1,4}){1,5})|((:[0-9A-Fa-f]{1,4}){0,3}:((25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)(\.(25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)){3}))|:))|(([0-9A-Fa-f]{1,4}:){1}(((:[0-9A-Fa-f]{1,4}){1,6})|((:[0-9A-Fa-f]{1,4}){0,4}:((25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)(\.(25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)){3}))|:))|(:(((:[0-9A-Fa-f]{1,4}){1,7})|((:[0-9A-Fa-f]{1,4}){0,5}:((25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)(\.(25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)){3}))|:)))(%.+)?
  29. IPV4 (?<![0-9])(?:(?:[0-1]?[0-9]{1,2}|2[0-4][0-9]|25[0-5])[.](?:[0-1]?[0-9]{1,2}|2[0-4][0-9]|25[0-5])[.](?:[0-1]?[0-9]{1,2}|2[0-4][0-9]|25[0-5])[.](?:[0-1]?[0-9]{1,2}|2[0-4][0-9]|25[0-5]))(?![0-9])
  30. IP (?:%{IPV6}|%{IPV4})
  31. HOSTNAME \b(?:[0-9A-Za-z][0-9A-Za-z-]{0,62})(?:\.(?:[0-9A-Za-z][0-9A-Za-z-]{0,62}))*(\.?|\b)
  32. IPORHOST (?:%{IP}|%{HOSTNAME})
  33. HOSTPORT %{IPORHOST}:%{POSINT}
  34. 
  35. # paths
  36. PATH (?:%{UNIXPATH}|%{WINPATH})
  37. UNIXPATH (/([\w_%!$@:.,+~-]+|\\.)*)+
  38. TTY (?:/dev/(pts|tty([pq])?)(\w+)?/?(?:[0-9]+))
  39. WINPATH (?>[A-Za-z]+:|\\)(?:\\[^\\?*]*)+
  40. URIPROTO [A-Za-z]+(\+[A-Za-z+]+)?
  41. URIHOST %{IPORHOST}(?::%{POSINT:port})?
  42. # uripath comes loosely from RFC1738, but mostly from what Firefox
  43. # doesn't turn into %XX
  44. URIPATH (?:/[A-Za-z0-9$.+!*'(){},~:;=@#%&_\-]*)+
  45. #URIPARAM \?(?:[A-Za-z0-9]+(?:=(?:[^&]*))?(?:&(?:[A-Za-z0-9]+(?:=(?:[^&]*))?)?)*)?
  46. URIPARAM \?[A-Za-z0-9$.+!*'|(){},~@#%&/=:;_?\-\[\]<>]*
  47. URIPATHPARAM %{URIPATH}(?:%{URIPARAM})?
  48. URI %{URIPROTO}://(?:%{USER}(?::[^@]*)?@)?(?:%{URIHOST})?(?:%{URIPATHPARAM})?
  49. 
  50. # Months: January, Feb, 3, 03, 12, December
  51. MONTH \b(?:[Jj]an(?:uary|uar)?|[Ff]eb(?:ruary|ruar)?|[Mm](?:a|ä)?r(?:ch|z)?|[Aa]pr(?:il)?|[Mm]a(?:y|i)?|[Jj]un(?:e|i)?|[Jj]ul(?:y)?|[Aa]ug(?:ust)?|[Ss]ep(?:tember)?|[Oo](?:c|k)?t(?:ober)?|[Nn]ov(?:ember)?|[Dd]e(?:c|z)(?:ember)?)\b
  52. MONTHNUM (?:0?[1-9]|1[0-2])
  53. MONTHNUM2 (?:0[1-9]|1[0-2])
  54. MONTHDAY (?:(?:0[1-9])|(?:[12][0-9])|(?:3[01])|[1-9])
  55. 
  56. # Days: Monday, Tue, Thu, etc...
  57. DAY (?:Mon(?:day)?|Tue(?:sday)?|Wed(?:nesday)?|Thu(?:rsday)?|Fri(?:day)?|Sat(?:urday)?|Sun(?:day)?)
  58. 
  59. # Years?
  60. YEAR (?>\d\d){1,2}
  61. HOUR (?:2[0123]|[01]?[0-9])
  62. MINUTE (?:[0-5][0-9])
  63. # '60' is a leap second in most time standards and thus is valid.
  64. SECOND (?:(?:[0-5]?[0-9]|60)(?:[:.,][0-9]+)?)
  65. TIME (?!<[0-9])%{HOUR}:%{MINUTE}(?::%{SECOND})(?![0-9])
  66. # datestamp is YYYY/MM/DD-HH:MM:SS.UUUU (or something like it)
  67. DATE_US %{MONTHNUM}[/-]%{MONTHDAY}[/-]%{YEAR}
  68. DATE_EU %{MONTHDAY}[./-]%{MONTHNUM}[./-]%{YEAR}
  69. ISO8601_TIMEZONE (?:Z|[+-]%{HOUR}(?::?%{MINUTE}))
  70. ISO8601_SECOND (?:%{SECOND}|60)
  71. TIMESTAMP_ISO8601 %{YEAR}-%{MONTHNUM}-%{MONTHDAY}[T ]%{HOUR}:?%{MINUTE}(?::?%{SECOND})?%{ISO8601_TIMEZONE}?
  72. DATE %{DATE_US}|%{DATE_EU}
  73. DATESTAMP %{DATE}[- ]%{TIME}
  74. TZ (?:[APMCE][SD]T|UTC)
  75. DATESTAMP_RFC822 %{DAY} %{MONTH} %{MONTHDAY} %{YEAR} %{TIME} %{TZ}
  76. DATESTAMP_RFC2822 %{DAY}, %{MONTHDAY} %{MONTH} %{YEAR} %{TIME} %{ISO8601_TIMEZONE}
  77. DATESTAMP_OTHER %{DAY} %{MONTH} %{MONTHDAY} %{TIME} %{TZ} %{YEAR}
  78. DATESTAMP_EVENTLOG %{YEAR}%{MONTHNUM2}%{MONTHDAY}%{HOUR}%{MINUTE}%{SECOND}
  79. 
  80. # Syslog Dates: Month Day HH:MM:SS
  81. SYSLOGTIMESTAMP %{MONTH} +%{MONTHDAY} %{TIME}
  82. PROG [\x21-\x5a\x5c\x5e-\x7e]+
  83. SYSLOGPROG %{PROG:program}(?:\[%{POSINT:pid}\])?
  84. SYSLOGHOST %{IPORHOST}
  85. SYSLOGFACILITY <%{NONNEGINT:facility}.%{NONNEGINT:priority}>
  86. HTTPDATE %{MONTHDAY}/%{MONTH}/%{YEAR}:%{TIME} %{INT}
  87. 
  88. # Shortcuts
  89. QS %{QUOTEDSTRING}
  90. 
  91. # Log formats
  92. SYSLOGBASE %{SYSLOGTIMESTAMP:timestamp} (?:%{SYSLOGFACILITY} )?%{SYSLOGHOST:logsource} %{SYSLOGPROG}:
  93. 
  94. # Log Levels
  95. LOGLEVEL ([Aa]lert|ALERT|[Tt]race|TRACE|[Dd]ebug|DEBUG|[Nn]otice|NOTICE|[Ii]nfo|INFO|[Ww]arn?(?:ing)?|WARN?(?:ING)?|[Ee]rr?(?:or)?|ERR?(?:OR)?|[Cc]rit?(?:ical)?|CRIT?(?:ICAL)?|[Ff]atal|FATAL|[Ss]evere|SEVERE|EMERG(?:ENCY)?|[Ee]merg(?:ency)?)
  96. HTTPDUSER %{EMAILADDRESS}|%{USER}
  97. HTTPDERROR_DATE %{DAY} %{MONTH} %{MONTHDAY} %{TIME} %{YEAR}
  98. 
  99. # Log formats
  100. HTTPD_COMMONLOG %{IPORHOST:clientip} %{HTTPDUSER:ident} %{HTTPDUSER:auth} \[%{HTTPDATE:timestamp}\] "(?:%{WORD:verb} %{NOTSPACE:request}(?: HTTP/%{NUMBER:httpversion})?|%{DATA:rawrequest})" %{NUMBER:response} (?:%{NUMBER:bytes}|-)
  101. HTTPD_COMBINEDLOG %{HTTPD_COMMONLOG} %{QS:referrer} %{QS:agent}
  102. 
  103. # Error logs
  104. HTTPD20_ERRORLOG \[%{HTTPDERROR_DATE:timestamp}\] \[%{LOGLEVEL:loglevel}\] (?:\[client %{IPORHOST:clientip}\] ){0,1}%{GREEDYDATA:message}
  105. HTTPD24_ERRORLOG \[%{HTTPDERROR_DATE:timestamp}\] \[%{WORD:module}:%{LOGLEVEL:loglevel}\] \[pid %{POSINT:pid}:tid %{NUMBER:tid}\]( \(%{POSINT:proxy_errorcode}\)%{DATA:proxy_message}:)?( \[client %{IPORHOST:clientip}:%{POSINT:clientport}\])? %{DATA:errorcode}: %{GREEDYDATA:message}
  106. HTTPD_ERRORLOG %{HTTPD20_ERRORLOG}|%{HTTPD24_ERRORLOG}
  107. 
  108. # Deprecated
  109. COMMONAPACHELOG %{HTTPD_COMMONLOG}
  110. COMBINEDAPACHELOG %{HTTPD_COMBINEDLOG}JAVACLASS (?:[a-zA-Z$_][a-zA-Z$_0-9]*\.)*[a-zA-Z$_][a-zA-Z$_0-9]*
  111. #Space is an allowed character to match special cases like 'Native Method' or 'Unknown Source'
  112. JAVAFILE (?:[A-Za-z0-9_. -]+)
  113. #Allow special <init>, <clinit> methods
  114. JAVAMETHOD (?:(<(?:cl)?init>)|[a-zA-Z$_][a-zA-Z$_0-9]*)
  115. #Line number is optional in special cases 'Native method' or 'Unknown source'
  116. JAVASTACKTRACEPART %{SPACE}at %{JAVACLASS:class}\.%{JAVAMETHOD:method}\(%{JAVAFILE:file}(?::%{NUMBER:line})?\)
  117. # Java Logs
  118. JAVATHREAD (?:[A-Z]{2}-Processor[\d]+)
  119. JAVACLASS (?:[a-zA-Z0-9-]+\.)+[A-Za-z0-9$]+
  120. JAVAFILE (?:[A-Za-z0-9_.-]+)
  121. JAVALOGMESSAGE (.*)
  122. # MMM dd, yyyy HH:mm:ss eg: Jan 9, 2014 7:13:13 AM
  123. CATALINA_DATESTAMP %{MONTH} %{MONTHDAY}, 20%{YEAR} %{HOUR}:?%{MINUTE}(?::?%{SECOND}) (?:AM|PM)
  124. # yyyy-MM-dd HH:mm:ss,SSS ZZZ eg: 2014-01-09 17:32:25,527 -0800
  125. TOMCAT_DATESTAMP 20%{YEAR}-%{MONTHNUM}-%{MONTHDAY} %{HOUR}:?%{MINUTE}(?::?%{SECOND}) %{ISO8601_TIMEZONE}
  126. CATALINALOG %{CATALINA_DATESTAMP:timestamp} %{JAVACLASS:class} %{JAVALOGMESSAGE:logmessage}
  127. # 2014-01-09 20:03:28,269 -0800 | ERROR | com.example.service.ExampleService - something compeletely unexpected happened...
  128. TOMCATLOG %{TOMCAT_DATESTAMP:timestamp} \| %{LOGLEVEL:level} \| %{JAVACLASS:class} - %{JAVALOGMESSAGE:logmessage}
  129. SYSLOG5424PRINTASCII [!-~]+
  130. 
  131. SYSLOGBASE2 (?:%{SYSLOGTIMESTAMP:timestamp}|%{TIMESTAMP_ISO8601:timestamp8601}) (?:%{SYSLOGFACILITY} )?%{SYSLOGHOST:logsource}+(?: %{SYSLOGPROG}:|)
  132. SYSLOGPAMSESSION %{SYSLOGBASE} (?=%{GREEDYDATA:message})%{WORD:pam_module}\(%{DATA:pam_caller}\): session %{WORD:pam_session_state} for user %{USERNAME:username}(?: by %{GREEDYDATA:pam_by})?
  133. 
  134. CRON_ACTION [A-Z ]+
  135. CRONLOG %{SYSLOGBASE} \(%{USER:user}\) %{CRON_ACTION:action} \(%{DATA:message}\)
  136. 
  137. SYSLOGLINE %{SYSLOGBASE2} %{GREEDYDATA:message}
  138. 
  139. # IETF 5424 syslog(8) format (see http://www.rfc-editor.org/info/rfc5424)
  140. SYSLOG5424PRI <%{NONNEGINT:syslog5424_pri}>
  141. SYSLOG5424SD \[%{DATA}\]+
  142. SYSLOG5424BASE %{SYSLOG5424PRI}%{NONNEGINT:syslog5424_ver} +(?:%{TIMESTAMP_ISO8601:syslog5424_ts}|-) +(?:%{IPORHOST:syslog5424_host}|-) +(-|%{SYSLOG5424PRINTASCII:syslog5424_app}) +(-|%{SYSLOG5424PRINTASCII:syslog5424_proc}) +(-|%{SYSLOG5424PRINTASCII:syslog5424_msgid}) +(?:%{SYSLOG5424SD:syslog5424_sd}|-|)
  143. 
  144. SYSLOG5424LINE %{SYSLOG5424BASE} +%{GREEDYDATA:syslog5424_msg}
  145. MAVEN_VERSION (?:(\d+)\.)?(?:(\d+)\.)?(\*|\d+)(?:[.-](RELEASE|SNAPSHOT))?
  146. MONGO_LOG %{SYSLOGTIMESTAMP:timestamp} \[%{WORD:component}\] %{GREEDYDATA:message}
  147. MONGO_QUERY \{ (?<={ ).*(?= } ntoreturn:) \}
  148. MONGO_SLOWQUERY %{WORD} %{MONGO_WORDDASH:database}\.%{MONGO_WORDDASH:collection} %{WORD}: %{MONGO_QUERY:query} %{WORD}:%{NONNEGINT:ntoreturn} %{WORD}:%{NONNEGINT:ntoskip} %{WORD}:%{NONNEGINT:nscanned}.*nreturned:%{NONNEGINT:nreturned}..+ (?<duration>[0-9]+)ms
  149. MONGO_WORDDASH \b[\w-]+\b
  150. MONGO3_SEVERITY \w
  151. MONGO3_COMPONENT %{WORD}|-
  152. MONGO3_LOG %{TIMESTAMP_ISO8601:timestamp} %{MONGO3_SEVERITY:severity} %{MONGO3_COMPONENT:component}%{SPACE}(?:\[%{DATA:context}\])? %{GREEDYDATA:message}
  153. # Default postgresql pg_log format pattern
  154. POSTGRESQL %{DATESTAMP:timestamp} %{TZ} %{DATA:user_id} %{GREEDYDATA:connection_id} %{POSINT:pid}
