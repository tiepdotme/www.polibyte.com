---
layout:     post
title:      Opensecrets api
---


While playing with OpenSecrets [web service](http://www.opensecrets.org/action/api_doc.php), I've run into some puzzling discrepancies between the data it returns and the data listed on the website. I'm interested in the PAC contributions from a given sector to a given candidate during a given cycle.

When I access [Senator Clintons 2006 PAC contributions](http://www.opensecrets.org/politicians/pacs.php?cycle=2006&cid=N00000019), I see $282,600 in Finance, Insurance & Real Estate PAC contributions. When I access this data through the candSector method of their API I get a response that lists $363,464 in Finance, Insurance & Real Estate PAC contributions. 


I tried using the CandIndByInd method to gather data on the specific industries that make up the Finance, Insurance & Real Estate sector. These numbers match when I receive them, but I sometimes get a "Resource not Found" error. For example [Senator Shelby's 2006 Finance, Insurance & Real Estate PAC contributions](http://www.opensecrets.org/politicians/pacs.php?cycle=2006&cid=N00009920&sector=F&seclong=Finance%2C+Insurance+%26+Real+Estate&newMem=N) shows $4,500 in misc finance contributions, but the API request just returns the error.





