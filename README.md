# Script-Cleaning-Data
library(textclean)
library(katadasaR)
library(tokenizers)
library(wordcloud)
library(dplyr)

tweets=read.csv(file.choose(), header=TRUE)
View(tweets)
tweets <- tweets$text %>% 
  as.character()
head(tweets)

tweets <- gsub( "\n"," ",tweets)
tweets[3]


tweets <- tweets %>% 
  replace_html() %>% # replace html with blank 
  replace_url()   # replace URLs with blank
tweets[199]


tweets <- tweets %>% 
  replace_emoji(.) %>% 
  replace_html(.)

tweets <- tweets %>% 
  replace_tag(tweets, pattern = "@([A-Za-z0-9_]+)",replacement="") %>%  # remove mentions
  replace_hash(tweets, pattern = "#([A-Za-z0-9_]+)",replacement="")      # remove hashtags

#print replaced text data on index [4:5]
tweets[4:5]

tweets<- gsub("RT :", "", tweets)
tweets[12]

coba=tweets[1:10]
coba

spell.lex <-read.csv(file.choose(), header=TRUE)
a=tweets[1:10]
#replace internet slang
tweets <- replace_internet_slang(a, slang = paste0("\\b",
                                                   spell.lex$slang, "\\b"),
                                 replacement = spell.lex$formal, ignore.case = TRUE)

#Often it is useful to remove all non relevant symbols and case from a text 
tweets <- strip(tweets)
tweets[1]

#delete uniq
tweets <- tweets %>% 
  as.data.frame() %>% 
  distinct()
View(tweets)
nrow(tweets)
write.csv(tweets,file = "D:\\Anis\\KULIAH\\DATA SEMESTERAN\\SKRIPSI\\tatapmuka1_cleaning.csv", row.names = FALSE)
