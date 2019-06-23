#IMPORTANT NOTES:
    #per https://dev.elsevier.com/api_key_settings.html, results are limited to 25 per API call. max is 200 - to increase to max, contact Elsevier
    #weekly request quota is 5000

#import the requisite packages
import requests
import json
import pandas as pd

#we are creating the complete API call by constructing the individual pieces below
#baseURL should change depending on the specific API call used. here we are accessing the author search API
baseURL = 'https://api.elsevier.com/content/'

#if you take a look at the Scopus API documentation, your endpoint should be readily apparent
endpoint = 'search/author'

#you can name this variable whatever you want. this piece is the actual query we will search on. update to fit your query accordingly
#in this example, we are searching on many author first and last name combinations
#we are also searching on subjareas and affiliation
#a note about affiliation - for about half of the sample author dataset Chelsea shared with me, 
#to find about half of the authors I had to remove the affiliation filter
query = '?query=((authlast(Last)%20and%20authfirst(First))+OR+(authlast(Last2)%20and%20authfirst(First2))+OR+(authlast(Last3)%20and%20authfirst(First3))+OR+(authlast(Last4)%20and%20authfirst(First4)))+AND+subjarea(busi+OR+econ+OR+deci+OR+comp+OR+psyc))'

#this is where you insert your respective API key
apikey = '&apikey=X'

#this tells the api that we would like our response to be in JSON, and that we want the maximum (200) responses
respFormat = '&httpAccept=application/json&count=200'

#this concatenates the above pieces of our call to look like one big web address
requestURL = baseURL + endpoint + query + apikey + respFormat

# Submit API request and import JSON response
apiRequest = requests.get(requestURL)
apiData = json.loads(apiRequest.text)

# Create data frame with column names
#the column names in single parentheses can be whatever you want as long as they correspond with your data
subjsearchauthordf = pd.DataFrame(columns=['Full Name', 'First Name', 'Last Name', 'Author ID', 'Affiliation ID', 'Affiliation Institution Country', 'Affiliation Institution City', 'Affiliation Name', 'Document Count'])

#loop over i authors
#comparatively elegant way to take each author's selected data and combine it into a single dataframe
for i in range(0,len(apiData['search-results']['entry'])):
    try:
        authorName = apiData['search-results']['entry'][i]['preferred-name']['given-name'] + " " + apiData['search-results']['entry'][i]['preferred-name']['surname']
        authorFirst = apiData['search-results']['entry'][i]['preferred-name']['given-name']
        authorLast = apiData['search-results']['entry'][i]['preferred-name']['surname']
        authorID = apiData['search-results']['entry'][i]['dc:identifier']
        affilID = apiData['search-results']['entry'][i]['affiliation-current']['affiliation-id']
        affilCountry = apiData['search-results']['entry'][i]['affiliation-current']['affiliation-country']
        affilCity = apiData['search-results']['entry'][i]['affiliation-current']['affiliation-city']
        affilName = apiData['search-results']['entry'][i]['affiliation-current']['affiliation-name']
        docCount = apiData['search-results']['entry'][i]['document-count']
        subjsearchauthordf = subjsearchauthordf.append(pd.Series([authorName, authorFirst, authorLast, authorID, affilID, affilCountry, affilCity, affilName, docCount], index=subjsearchauthordf.columns), ignore_index=True)
    except:
        pass

#below are diagnostics for this process

#this prints the dataframe we just made. this simply shows that we were successful in creating a df that looks like what we wanted
print(subjsearchauthordf)

#this tells us if the api request was successful. "200" means all is well
print(apiRequest)

#this prints the data converted from JSON, which simply shows that the data looks as expected
print(apiData)

#this shows the full request URL. this is how the request URL would look as a url in a web browser
print(requestURL)

#create a Pandas Excel writer using XlsxWriter as the engine
writer = pd.ExcelWriter('subjsearchauthordf52.xlsx', engine='xlsxwriter')

#convert the dataframe to an XlsxWriter Excel object
subjsearchauthordf.to_excel(writer,sheet_name='Sheet1')

#close the Pandas Excel writer and output the Excel file
writer.save()
