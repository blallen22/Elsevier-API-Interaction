#import the requisite packages
import requests
import json
import pandas as pd

#we are creating the complete API call by constructing the individual pieces below
#baseURL should change depending on the specific API call used. here we are accessing the author retrieval API
baseURL = 'https://api.elsevier.com/'

#if you take a look at the Scopus API documentation, your endpoint should be readily apparent
endpoint = 'content/author'

#insert the author IDs separated by commas
authorID = '&author_id=X'

#adding view=ENHANCED to the query allows us to access all variables in an author's profile
query = '?view=ENHANCED'

#this is where you insert your respective API key
apikey = '&apikey=X'

#this tells the api that we would like our response to be in JSON, please and thank you
respFormat = '&httpAccept=application/json&count=200'

#this concatenates the above pieces of our call to look like one big web address
requestURL = baseURL + endpoint + query + authorID + apikey + respFormat

# Submit API request and import JSON response
apiRequest = requests.get(requestURL)
apiData = json.loads(apiRequest.text)

# Create data frame with column names
df = pd.DataFrame(columns=['Author Name', 'Author First Name', 'Author Last Name', 'Author ID', 'Current Affiliation ID', 'Current Affiliation Full Name',  'Current Affiliation City', 'Current Affiliation Country', 'First Publication Year', 'Most Recent Publication Year', 'Doc Count', 'Citation Count', 'Cited By Count', 'h-index'])
 

#loop over i authors
#comparatively elegant way to take each author's selected data and combine it into a single dataframe
for i in range(0,len(apiData['author-retrieval-response-list']['author-retrieval-response'])):
    try:
        authorName = apiData['author-retrieval-response-list']['author-retrieval-response'][i]['author-profile']['preferred-name']['given-name'] + " " + apiData['author-retrieval-response-list']['author-retrieval-response'][i]['author-profile']['preferred-name']['surname']
    except:
        pass
    try:
        authorFirst = apiData['author-retrieval-response-list']['author-retrieval-response'][i]['author-profile']['preferred-name']['given-name']
    except:
        pass
    try:
        authorLast = apiData['author-retrieval-response-list']['author-retrieval-response'][i]['author-profile']['preferred-name']['surname']
    except:
        pass
    try:
        authorID = apiData['author-retrieval-response-list']['author-retrieval-response'][i]['coredata']['dc:identifier']
    except:
        pass
    try:
        currentaffilID = apiData['author-retrieval-response-list']['author-retrieval-response'][i]['author-profile']['affiliation-current']['affiliation']['ip-doc']['@id']
    except:
        pass
    try:
        currentaffilfullname = apiData['author-retrieval-response-list']['author-retrieval-response'][i]['author-profile']['affiliation-current']['affiliation']['ip-doc']['afdispname']
    except:
        pass
    try:
        currentaffilcity = apiData['author-retrieval-response-list']['author-retrieval-response'][i]['author-profile']['affiliation-current']['affiliation']['ip-doc']['address']['city']
    except:
        pass
    try:
        currentaffilcountry = apiData['author-retrieval-response-list']['author-retrieval-response'][i]['author-profile']['affiliation-current']['affiliation']['ip-doc']['address']['country']
    except:
        pass
    try:
        firstpubyear = apiData['author-retrieval-response-list']['author-retrieval-response'][i]['author-profile']['publication-range']['@start']
    except:
        pass
    try:
        mostrecentpubyear = apiData['author-retrieval-response-list']['author-retrieval-response'][i]['author-profile']['publication-range']['@end']
    except:
        pass
    try:
        docCount = apiData['author-retrieval-response-list']['author-retrieval-response'][i]['coredata']['document-count']
    except:
        pass
    try:
        citationCount = apiData['author-retrieval-response-list']['author-retrieval-response'][i]['coredata']['citation-count']
    except:
        pass
    try:
        citedbyCount = apiData['author-retrieval-response-list']['author-retrieval-response'][i]['coredata']['cited-by-count']
    except:
        pass
    try:
        hIndex = apiData['author-retrieval-response-list']['author-retrieval-response'][i]['h-index']
    except:
        pass
    try:
        df = df.append(pd.Series([authorName, authorFirst, authorLast, authorID, currentaffilID, currentaffilfullname, currentaffilcity, currentaffilcountry, firstpubyear, mostrecentpubyear, docCount, citationCount, citedbyCount, hIndex], index=df.columns), ignore_index=True)
    except:
        pass

#below are diagnostics for throughout this process

#this prints the dataframe we just made. this simply shows that we were successful in creating a df that looks like what we wanted
print(df)

#this tells us if the api request was successful. "200" means all is well
print(apiRequest)

#this prints the data converted from JSON, which simply shows that the data looks as we expected
print(apiData)

#this shows the full request URL. this is how the request URL would look as a url in a web browser
print(requestURL)

#create a Pandas Excel writer using XlsxWriter as the engine
writer = pd.ExcelWriter('df39.xlsx', engine='xlsxwriter')

#convert the dataframe to an XlsxWriter Excel object
df.to_excel(writer,sheet_name='Sheet1')

#close the Pandas Excel writer and output the Excel file
writer.save()
