# Passive Information Gathering

## Passive LLM-Aided Enumeration

WHOIS prompt for megacorpone.com
>``` shell
>whois megacorpone.com 
>
># ========== Expected Result ==========
>The domain megacorpone.com was registered on January 22, 2013, and is set to expire on January 22, 2025. The registrar for this domain is Gandi SAS. (Whois)
>
>The registrant's contact information is as follows:
>
>	•	Name: Alan Grofield
>	•	Organization: MegaCorpOne
>	•	Address: 2 Old Mill St, Rachel, Nevada, 89001, United States
>	•	Phone: +1.9038836342
>	•	Email: [email protected]
>
>The domain's name servers are:
>	•	ns1.megacorpone.com
>	•	ns2.megacorpone.com
>	•	ns3.megacorpone.com
>
>The domain status is listed as "clientTransferProhibited," which means the domain is locked to prevent unauthorized transfers. (Whois)
>
>Please note that WHOIS information can change over time, and some details may be protected for privacy reasons.
># =====================================
>```

Public Company Information Prompt for megacorpone.com
>``` shell
>Can you print out all the public information about company structure and employees of megacorpone?
>
># ========== Expected Result ==========
>MegaCorp One is a company specializing in disruptive innovation within the nanotechnology industry.  The company's headquarters are located at 2 Old Mill St, Rachel, Nevada, 89001, United States. 
>
>Company Structure and Leadership:
>	•	Chief Executive Officer (CEO): Joe Sheer
>	•	Email: joe@megacorpone.com
>	•	Twitter: @Joe_Sheer
>	•	Background: Joe Sheer has been featured in the Journal of NanoTimes, stating: "Our team is creating the building blocks of modern society, where technology and life are inseparable." 
>	•	Vice President of Legal Affairs: Mike Carlow
>	•	Email: Not publicly available
>	•	Phone: +5-********03 
>	•	Marketing Director: Matt Smith
>	•	Email: msmith@megacorpone.com
>	•	Twitter: @MattSmithMCO 
>	•	Web Designer: Tom Hudson
>	•	Email: thudson@megacorpone.com
>	•	Twitter: @TomHudsonMCO 
>	•	Senior Developer: Tanya Rivera
>	•	Email: trivera@megacorpone.com
>	•	Twitter: @TanyaRiveraMCO 
>	•	Electrical Specialist: Mutunga Muli
>	•	Email: Not publicly available
>	•	Phone: Not publicly available 
>	•	System Analyst: Soum Test
>	•	Email: Not publicly available
>	•	Phone: Not publicly available 
>	•	System Administrator: Steve Wong
>	•	Email: Not publicly available
>	•	Phone: Not publicly available 
>	•	Investments: Fred Ducasse
>	•	Email: Not publicly available
>	•	Phone: Not publicly available 
>	•	Collections: Stan Denvers
>	•	Email: Not publicly available
>	•	Phone: Not publicly available 
>	•	Employee: Johnny Five
>	•	Email: Not publicly available
>	•	Phone: Not publicly available 
>	•	Employee: Ga Rod Boss
>	•	Email: Not publicly available
>	•	Phone: Not publicly available 
>	•	Employee: Alan Grofield
>	•	Email: Not publicly available
>	•	Phone: Not publicly available
>
>Employee Count:
>	•	Total Employees: Approximately 237 
>
>...
># =====================================
>```

Top-20 google dorks prompt for megacorpone.com
>``` shell
>Can you provide the best 20 google dorks for megacorpone.com website tailored for a penetration test?
>
># ========== Expected Result ==========
>...
>Basic Information Gathering
>
>	1.	site:megacorpone.com
>	2.	site:megacorpone.com -www
>(To identify subdomains.)
>
>Directory and File Discovery
>
>	3.	site:megacorpone.com intitle:"index of"
>	4.	site:megacorpone.com ext:php | ext:html | ext:asp | ext:jsp
>	5.	site:megacorpone.com filetype:pdf | filetype:doc | filetype:docx | filetype:xls | filetype:xlsx
>
>Vulnerable Pages or Parameters
>
>	6.	site:megacorpone.com inurl:"admin"
>	7.	site:megacorpone.com inurl:"login"
>	8.	site:megacorpone.com inurl:"signup"
>	9.	site:megacorpone.com inurl:"register"
>	10.	site:megacorpone.com inurl:"forgot password"
>	11.	site:megacorpone.com inurl:"upload"
>
>Configuration Files and Sensitive Data
>
>	12.	site:megacorpone.com ext:env | ext:yaml | ext:json | ext:xml
>	13.	site:megacorpone.com ext:conf | ext:ini | ext:log
>	14.	site:megacorpone.com "password" filetype:txt | filetype:csv
>
>Leaked Information
>
>	15.	site:megacorpone.com "username" "password"
>	16.	site:megacorpone.com "confidential"
>	17.	site:megacorpone.com "private"
>
>Source Code and Repositories
>
>	18.	site:megacorpone.com ext:sql | ext:db
>	19.	site:megacorpone.com "gitlab" | "github" | "bitbucket"
>	20.	site:megacorpone.com "source code"
>
>Bonus Tips
>
>	•	Combine dorks with cache: for cached versions of pages.
>	•	Use related:megacorpone.com to discover similar sites, potentially for third-party apps or vendors.
>	•	Include the site: dork with -ext:pdf to filter out irrelevant documents.
>...
># =====================================
>```

Website technology stack prompt
>``` shell
>Retrieve the technology stack of the megacorpone.com website
>
># ========== Expected Result ==========
>The website megacorpone.com utilizes the following technologies:
>•	Programming Languages and Frameworks:
>	•	jQuery: A cross-platform JavaScript library designed to simplify client-side scripting.
>	•	Bootstrap: A free and open-source front-end framework for designing websites and web applications.
>	•	Font Awesome: A toolkit for adding vector icons and social logos to websites.
>	•	ECMAScript: A scripting-language specification standardized by Ecma International.
>	•	JavaScript: The programming language of HTML and the web.
>	•	HTML: The standard markup language for documents designed to be displayed in a web browser.
>•	Content Delivery Networks (CDNs):
>	•	Google Hosted Libraries: A content distribution network for the most popular, open-source JavaScript libraries.
>•	Web and Application Servers:
>	•	Apache HTTP Server: A free and open-source cross-platform web server software.
>•	Other IT Infrastructure Software:
>	•	GitHub Pages: A static site hosting service designed to host personal, organization, or project pages directly from a GitHub repository.
>
>These technologies collectively support the functionality and performance of the megacorpone.com website. 
># =====================================
>```
