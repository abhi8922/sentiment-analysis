# sentiment-analysis
Sentiment Analysis System
What: Natural Language Processing 
1.	It is a Natural Language Processing Application which can analize the sentiment on a text data.
2.	This application predicts the sentiment into 3 categories, Positive, Negative and Neutral.
3.	This application can visualize the results based on different factors such age, gender, language, city.
4.	This application can acquire data from google forms through google sheet
5.	This application is a web application which can be accesed over a LAN
Why:
1.	This project has many real life application and is used in such as product/service monitoring, survey analysis, social media monitoring feedback analysis.
2.	This progect shows your programming skills, Machine Learning Knowledge, practical implementation of Natural Language Processing.
3.	This kind of project is very good from resume point of view
How:
1.	Backend : Data Collection : Google Sheet with Python
    Data Organization: Pandas
    Data Analysis : NLTK, vaderSentiment
    Data Visualization : ploty
2.	Frontend : Google Forms
                    Web Application : Streamlit















Backend
Google Form And Sheet


Google Account
Google Project
Enable Google Sheet API
Create a consent application
Download Credential
Google Account Authentication
 
Python Code Snippet:

from google_auth_oauthlib.flow import InstalledAppFlow
from googleapiclient.discovery import build
#Permission
f=InstalledAppFlow.from_client_secrets_file("key.json",["https://www.googleapis.com/auth/spreadsheets"])
cred=f.run_local_server(port=0)
service=build("Sheets","v4",credentials=cred).spreadsheets().values()
d=service.get(spreadsheetId="1IHqijN5m1AuvkFv5F93okItjwx1ZR2dX-Wya5ULvnXk",range="A:F").execute()
data=d['values']
print(data)

Google Spread Sheet
 

from google_auth_oauthlib.flow import InstalledAppFlow
from googleapiclient.discovery import build
import pandas as pd
#Permission
f=InstalledAppFlow.from_client_secrets_file("key.json",["https://www.googleapis.com/auth/spreadsheets"])
cred=f.run_local_server(port=0)
service=build("Sheets","v4",credentials=cred).spreadsheets().values()
k=service.get(spreadsheetId="1IHqijN5m1AuvkFv5F93okItjwx1ZR2dX-Wya5ULvnXk",range="A:F").execute()
d=k['values']
df=pd.DataFrame(data=d[1:],columns=d[0])
for i in range(0,len(df)):
    t=df._get_value(i,"Opinion")
    print(t)










Sentiment Analyzer
 
 
Python Code Snippet

from vaderSentiment.vaderSentiment import SentimentIntensityAnalyzer
mymodel=SentimentIntensityAnalyzer()
k=input("Enter the Text")
pred=mymodel.polarity_scores(k)
if (pred['compound']>0.5):
    print("Sentiment is Positive")
elif(pred['compound']<-0.5):
    print("Sentiment is Negative")
else:
    print("Sentiment is Neutral")
Saving Data Locally
 


from google_auth_oauthlib.flow import InstalledAppFlow
from googleapiclient.discovery import build
import pandas as pd
from vaderSentiment.vaderSentiment import SentimentIntensityAnalyzer
mymodel=SentimentIntensityAnalyzer()
#Permission
f=InstalledAppFlow.from_client_secrets_file("key.json",["https://www.googleapis.com/auth/spreadsheets"])
cred=f.run_local_server(port=0)
service=build("Sheets","v4",credentials=cred).spreadsheets().values()
k=service.get(spreadsheetId="1IHqijN5m1AuvkFv5F93okItjwx1ZR2dX-Wya5ULvnXk",range="B:F").execute()
d=k['values']
df=pd.DataFrame(data=d[1:],columns=d[0])
l=[]
for i in range(0,len(df)):
    t=df._get_value(i,"Opinion")
    pred=mymodel.polarity_scores(t)
    if (pred['compound']>0.5):
        l.append("Positive")
    elif(pred['compound']<-0.5):
        l.append("Negative")
    else:
        l.append("Neutral")
df['Sentiment']=l
df.to_csv("results.csv",index=False)

Data Visualistion
Pie Chart:
 
Python Code Snippet
import pandas as pd
import plotly.express as px
df=pd.read_csv("results.csv")
posper=(len(df[df['Sentiment']=='Positive'])/len(df))*100
negper=(len(df[df['Sentiment']=='Negative'])/len(df))*100
neuper=(len(df[df['Sentiment']=='Neutral'])/len(df))*100
fig=px.pie(values=[posper,negper,neuper],names=['Positive','Negative','Neutral'])
fig.show()










Scatter Plot:
 
Python Code Snippet
import pandas as pd
import plotly.express as px
df=pd.read_csv("results.csv")
fig=px.scatter(x=df['Age'],y=df['Gender'],color=df['Sentiment'],size=df['Age'])
fig.show()

Histogram:
 

Python Code Snippet:
import pandas as pd
import plotly.express as px
df=pd.read_csv("results.csv")
fig=px.histogram(x=df['Gender'],color=df['Sentiment'])
fig.show()

Saving Data into Google Sheet
 
Python Code Snippet:
from google_auth_oauthlib.flow import InstalledAppFlow
from googleapiclient.discovery import build
import pandas as pd
from vaderSentiment.vaderSentiment import SentimentIntensityAnalyzer
mymodel=SentimentIntensityAnalyzer()
#Permission
f=InstalledAppFlow.from_client_secrets_file("key.json",["https://www.googleapis.com/auth/spreadsheets"])
cred=f.run_local_server(port=0)
service=build("Sheets","v4",credentials=cred).spreadsheets().values()
k=service.get(spreadsheetId="1IHqijN5m1AuvkFv5F93okItjwx1ZR2dX-Wya5ULvnXk",range="B:F").execute()
d=k['values']
df=pd.DataFrame(data=d[1:],columns=d[0])
l=[]
for i in range(0,len(df)):
    t=df._get_value(i,"Opinion")
    pred=mymodel.polarity_scores(t)
    if (pred['compound']>0.5):
       d[i+1].append("Positive")
    elif(pred['compound']<-0.5):
        d[i+1].append("Negative")
    else:
        d[i+1].append("Neutral")
h={'values':d}
k=service.update(spreadsheetId="1IHqijN5m1AuvkFv5F93okItjwx1ZR2dX-Wya5ULvnXk",range="B:G",valueInputOption="USER_ENTERED",body=h).execute()

Front End
     

Python Code Snippet:
import streamlit as st
from google_auth_oauthlib.flow import InstalledAppFlow
from googleapiclient.discovery import build
from vaderSentiment.vaderSentiment import SentimentIntensityAnalyzer
import pandas as pd
import plotly.express as px
st.set_page_config(page_title="Sentiment Analysis System",page_icon="data:image/png;base64,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")
st.title("SENTIMENT ANALYSIS SYSTEM")
choice=st.sidebar.selectbox("My Menu",("HOME","ANALYSIS","RESULTS"))
if (choice=="HOME"):
    st.image("https://cdn.dribbble.com/users/113062/screenshots/2812589/bots.gif")
    st.write("1. It is a Natural Language Processing Application which can analize the sentiment on a text data.")
    st.write("2. This application predicts the sentiment into 3 categories, Positive, Negative and Neutral.")
    st.write("3. This application can visualize the results based on different factors such age, gender, language, city.")
elif(choice=="ANALYSIS"):
    sid=st.text_input("Enter Google Sheet ID")
    r=st.text_input("Enter Range between first column and last column")
    c=st.text_input("Enter column name tat is to be analized")
    btn=st.button("Analyze")
    if btn:
        if 'cred' not in st.session_state:
            f=InstalledAppFlow.from_client_secrets_file("key.json",["https://www.googleapis.com/auth/spreadsheets"])
            st.session_state['cred']=f.run_local_server(port=0)
        mymodel=SentimentIntensityAnalyzer()
        service=build("Sheets","v4",credentials=st.session_state['cred']).spreadsheets().values()
        k=service.get(spreadsheetId=sid,range=r).execute()
        d=k['values']
        df=pd.DataFrame(data=d[1:],columns=d[0])
        l=[]
        for i in range(0,len(df)):
            t=df._get_value(i,"Opinion")
            pred=mymodel.polarity_scores(t)
            if (pred['compound']>0.5):
                l.append("Positive")
            elif(pred['compound']<-0.5):
                l.append("Negative")
            else:
                l.append("Neutral")
        df['Sentiment']=l
        df.to_csv("results.csv",index=False)
        st.subheader("The Analysis results are saved by the name of results.csv file")
elif(choice=="RESULTS"):
    df=pd.read_csv("results.csv")
    choice2=st.selectbox("Chose Visualisation",("NONE","PIE CHART","HISTOGRAM","SCATTER PLOT"))
    st.dataframe(df)
    if (choice2=="PIE CHART"):
        posper=(len(df[df['Sentiment']=='Positive'])/len(df))*100
        negper=(len(df[df['Sentiment']=='Negative'])/len(df))*100
        neuper=(len(df[df['Sentiment']=='Neutral'])/len(df))*100
        fig=px.pie(values=[posper,negper,neuper],names=['Positive','Negative','Neutral'])
        st.plotly_chart(fig)
    elif(choice2=="HISTOGRAM"):
        k=st.selectbox("Choose Column",df.columns)
        if k:
            fig=px.histogram(x=df[k],color=df['Sentiment'])
            st.plotly_chart(fig)
    elif(choice2=="SCATTER PLOT"):
        k=st.text_input("Enter the continous column name")
        if k:
            fig=px.scatter(x=df[k],y=df['Sentiment'])
            st.plotly_chart(fig)
        
        

    

        


