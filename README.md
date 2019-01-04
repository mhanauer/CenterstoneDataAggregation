rm(list=ls())
library(foreign)
library(h2o)
library(plyr)



# Now GPRA Adult.  Need to rename the PARTID to ID.  Make sure you aggregate baseline with 3 month then with 6 month
#
setwd("S:/Indiana Research & Evaluation/CCPE/CCPE SPSS - Datasets/Baseline ADULT")
gpraAdultBase = read.csv("CCPE GRPA - Baseline.csv", header = TRUE) 
head(gpraAdultBase)


names(gpraAdultBase)[1] =  "INSTRMNT_LANG"


######################## Now get GPRA enrollment Redcap adult data ####################################
setwd("S:/Indiana Research & Evaluation/CCPE/CCPE SPSS - Datasets/Baseline ADULT")
gpraAdultBaselineRedCap = read.csv("CCPEAdultBaselineRedCap.csv", header = TRUE)

### Try to rbind the full by getting rid of the extra first 
setwd("S:/Indiana Research & Evaluation/CCPE/CCPE SPSS - Datasets/Baseline ADULT")
gpraAdultBaselineRedCapFull = read.csv("CCPEAdultBaselineRedCapFull.csv", header = TRUE)

dim(gpraAdultBaselineRedCap)
dim(gpraAdultBaselineRedCapFull)

gpraAdultBaselineRedCapFull = gpraAdultBaselineRedCapFull[c(1:152)]



gpraAdultBaselineRedCap = rbind(gpraAdultBaselineRedCap, gpraAdultBaselineRedCapFull)


gpraAdultBaselineRedCap$record_id = NULL
gpraAdultBaselineRedCap$redcap_survey_identifier = NULL
gpraAdultBaselineRedCap$national_minority_sahiv_prevention_initiative_adul_timestamp = NULL
gpraAdultBaselineRedCap$name = NULL
gpraAdultBaselineRedCap$national_minority_sahiv_prevention_initiative_adul_complete = NULL

gpraAdultBaselineRedCap = data.frame(gpraAdultBaselineRedCap)

names(gpraAdultBaselineRedCap) = toupper(names(gpraAdultBaselineRedCap))

# Need to get rid of the extra 0's and rename them
gpraAdultBaselineRedCap$DEPLOYEDIRAQ___0 = NULL
gpraAdultBaselineRedCap$DEPLOYEDPERS___0 = NULL
gpraAdultBaselineRedCap$DEPLOYEDASIA___0 = NULL
gpraAdultBaselineRedCap$DEPLOYEDKOR___0 = NULL
gpraAdultBaselineRedCap$DEPLOYEDWWII___0 = NULL
gpraAdultBaselineRedCap$DEPLOYEDOTH___0 = NULL
gpraAdultBaselineRedCap$RESPECT_RACE___0 = NULL
gpraAdultBaselineRedCap$RESPECT_REL___0 = NULL
gpraAdultBaselineRedCap$RESPECT_GENDER___0 = NULL
gpraAdultBaselineRedCap$RESPECT_AGE___0 = NULL
gpraAdultBaselineRedCap$RESPECT_SEXPR___0 = NULL
gpraAdultBaselineRedCap$RESPECT_DISABLE___0 = NULL
gpraAdultBaselineRedCap$RESPECT_MH___0 = NULL
gpraAdultBaselineRedCap$RESPECT_HIV___0 = NULL
gpraAdultBaselineRedCap$RESPECT_NONE___0 = NULL

head(gpraAdultBaselineRedCap)

gpraAdultBaselineRedCap = rename(gpraAdultBaselineRedCap, c("DEPLOYEDIRAQ___1" = "DEPLOYEDIRAQ", "DEPLOYEDPERS___1" = "DEPLOYEDPERS", "DEPLOYEDASIA___1" = "DEPLOYEDASIA", "DEPLOYEDKOR___1" = "DEPLOYEDKOR", "DEPLOYEDWWII___1" = "DEPLOYEDWWII", "DEPLOYEDOTH___1" = "DEPLOYEDOTH", "RESPECT_RACE___1" = "RESPECT_RACE", "RESPECT_REL___1" = "RESPECT_REL", "RESPECT_GENDER___1" = "RESPECT_GENDER", "RESPECT_AGE___1" = "RESPECT_AGE", "RESPECT_SEXPR___1" = "RESPECT_SEXPR", "RESPECT_DISABLE___1" = "RESPECT_DISABLE", "RESPECT_MH___1" = "RESPECT_MH", "RESPECT_HIV___1"= "RESPECT_HIV", "RESPECT_NONE___1" = "RESPECT_NONE"))



# Ok now need to add these variables with NA's INSTRMNT_LANG
# Ok for the first six people, we need to make intervention what ever CTR is and given them the same date. 9/26/18
#LANG_OTHER
#GRANT_ID
#DESIGNGRP
#MONTH
#DAY
#YEAR
#INTTYPE
#INTDUR
#INTERVENTION_A
#INTERVENTION_B
#INTERVENTION_C
#SIS = 2, CTR = 1
INSTRMNT_LANG = rep(1, dim(gpraAdultBaselineRedCap)[1])
LANG_OTHER = rep(NA, dim(gpraAdultBaselineRedCap)[1])
GRANT_ID = rep(NA, dim(gpraAdultBaselineRedCap)[1])
DESIGNGRP = rep(1, dim(gpraAdultBaselineRedCap)[1])
INTTYPE = rep(1, dim(gpraAdultBaselineRedCap)[1])
INTDUR = rep(3, dim(gpraAdultBaselineRedCap)[1])
PARTID = gpraAdultBaselineRedCap$PARTID
# Need to erase this, because it will mess up the order below
gpraAdultBaselineRedCap$PARTID = NULL
MONTH = gpraAdultBaselineRedCap$MONTH
DAY = gpraAdultBaselineRedCap$DAY
YEAR  = gpraAdultBaselineRedCap$YEAR

gpraAdultBaselineRedCap$MONTH = NULL
gpraAdultBaselineRedCap$DAY = NULL
gpraAdultBaselineRedCap$YEAR = NULL
gpraAdultBaselineRedCap$EMAIL_ADDRESS = NULL
gpraAdultBaselineRedCap$INTERVENTION_TYPE = NULL

INTERVENTION_A = rep(1, dim(gpraAdultBaselineRedCap)[1])
INTERVENTION_B = rep(NA, dim(gpraAdultBaselineRedCap)[1])
INTERVENTION_C = rep(NA, dim(gpraAdultBaselineRedCap)[1])

# So now combine the variables above 
gpraAdultBaselineRedCap = data.frame(INSTRMNT_LANG = INSTRMNT_LANG, LANG_OTHER = LANG_OTHER, GRANT_ID = GRANT_ID, DESIGNGRP = DESIGNGRP, PARTID = PARTID, MONTH =MONTH, DAY = DAY, YEAR = YEAR,  INTTYPE = INTTYPE, INTDUR = INTDUR, INTERVENTION_A = INTERVENTION_A, INTERVENTION_B = INTERVENTION_B, INTERVENTION_C = INTERVENTION_C,gpraAdultBaselineRedCap)
head(gpraAdultBaselineRedCap)
head(gpraAdultBase)
dim(gpraAdultBaselineRedCap)
dim(gpraAdultBase)

write.csv(gpraAdultBaselineRedCap, "gpraAdultBaselineRedCap.csv", row.names = FALSE)
# Now stack baseline data
gpraAdultBase = rbind(gpraAdultBase, gpraAdultBaselineRedCap)

dim(gpraAdultBase)
dim(gpraAdultBaselineRedCap)

#### Now get 3-month data

gpraAdult3month = read.spss("S:/Indiana Research & Evaluation/CCPE/CCPE SPSS - Datasets/3 Month Reassessments ADULT/Reassess 3M CCPE GPRA Adult.sav", use.value.labels = FALSE, to.data.frame = TRUE)
## Need to add the 3 month from the Red for the only GRPA and all assessments, but just drop all other assessments so everything after svytruth
setwd("S:/Indiana Research & Evaluation/CCPE/CCPE SPSS - Datasets/3 Month Reassessments ADULT")

gpraAdult3monthRedCap = read.csv("CCPEFollowupAdultQueRedCap.csv", header = TRUE)

gpraAdult3monthRedCap$redcap_data_access_group = NULL

gpraAdult3monthRedCapFull = read.csv("CCPEFollowupAdultQueRedCapFull.csv", header = TRUE)
# Need to get rid of extra for red cap full

gpraAdult3monthRedCapFull = gpraAdult3monthRedCapFull[,1:145]
gpraAdult3monthRedCap = gpraAdult3monthRedCap[,1:145]
gpraAdult3monthRedCapAll = rbind(gpraAdult3monthRedCapFull, gpraAdult3monthRedCap)
# Missing several variables and id value not the same
# First make the variables all upper case

names(gpraAdult3monthRedCapAll) = toupper(names(gpraAdult3monthRedCapAll))
# These don't match RECORD_ID REDCAP_SURVEY_IDENTIFIER NATIONAL_MINORITY_SAHIV_PREVENTION_INITIATIVE_ADUL_TIMESTAMP                 NAME
head(gpraAdult3monthRedCapAll)
#Subset only those REDCAP_SURVEY_IDENTIFIER with none na's
gpraAdult3monthRedCapAll = subset(gpraAdult3monthRedCapAll, REDCAP_SURVEY_IDENTIFIER > 0)
# Need to change these to just a year.

gpraAdult3monthRedCapAll$NATIONAL_MINORITY_SAHIV_PREVENTION_INITIATIVE_ADUL_TIMESTAMP
YEAR = data.frame(gpraAdult3monthRedCapAll$NATIONAL_MINORITY_SAHIV_PREVENTION_INITIATIVE_ADUL_TIMESTAMP)
colnames(YEAR) = c("YEAR")

# Need to get rid of RECORD_ID, NATIONAL_MINORITY_SAHIV_PREVENTION_INITIATIVE_ADUL_TIMESTAMP, NAME
gpraAdult3monthRedCapAll$RECORD_ID = NULL
gpraAdult3monthRedCapAll$NATIONAL_MINORITY_SAHIV_PREVENTION_INITIATIVE_ADUL_TIMESTAMP = NULL
gpraAdult3monthRedCapAll$NAME = NULL

# Need to add these variables INSTRMNT_LANG, LANG_OTHER, GRANT_ID, DESIGNGRP, MONTH, DAY, INTTYPE, INTDUR, INTERVENTION_A, INTERVENTION_B, INTERVENTION_C, YEAR
n = dim(gpraAdult3monthRedCapAll)[1]
n = rep(NA, n)
INSTRMNT_LANG = n
LANG_OTHER = n
GRANT_ID = n
DESIGNGRP = n
MONTH = n
DAY = n
YEAR = YEAR
INTTYPE =n
INTDUR = n
INTERVENTION_A =n 
INTERVENTION_B = n
INTERVENTION_C = n
gpraAdult3monthRedCapFirst = cbind(INSTRMNT_LANG, LANG_OTHER, GRANT_ID, DESIGNGRP, PARTID = gpraAdult3monthRedCapAll$REDCAP_SURVEY_IDENTIFIER, MONTH, DAY, YEAR, INTTYPE, INTDUR, INTERVENTION_A, INTERVENTION_B, INTERVENTION_C)
gpraAdult3monthRedCapAll$REDCAP_SURVEY_IDENTIFIER = NULL
dim(gpraAdult3monthRedCapFirst)
dim(gpraAdult3monthRedCapAll)
gpraAdult3monthRedCapAll = cbind(gpraAdult3monthRedCapFirst, gpraAdult3monthRedCapAll)
summary(gpraAdult3monthRedCapAll)

write.csv(gpraAdult3monthRedCapAll, "gpraAdult3monthRedCapAll.csv", row.names = FALSE)

# Now combine all the new variables in front of previous GPRA so it is the same order as before.  Should just rbind them to the 3 month reassessment.
# Double check duplicate IDs. 

gpraAdult3monthRedCapAll$DEPLOYEDIRAQ___0 = NULL
gpraAdult3monthRedCapAll$DEPLOYEDPERS___0 = NULL
gpraAdult3monthRedCapAll$DEPLOYEDASIA___0 = NULL
gpraAdult3monthRedCapAll$DEPLOYEDKOR___0 = NULL
gpraAdult3monthRedCapAll$DEPLOYEDWWII___0 = NULL
gpraAdult3monthRedCapAll$DEPLOYEDOTH___0 = NULL
gpraAdult3monthRedCapAll$RESPECT_RACE___0 = NULL
gpraAdult3monthRedCapAll$RESPECT_REL___0 = NULL
gpraAdult3monthRedCapAll$RESPECT_GENDER___0 = NULL
gpraAdult3monthRedCapAll$RESPECT_AGE___0 = NULL
gpraAdult3monthRedCapAll$RESPECT_SEXPR___0 = NULL
gpraAdult3monthRedCapAll$RESPECT_DISABLE___0 = NULL
gpraAdult3monthRedCapAll$RESPECT_MH___0 = NULL
gpraAdult3monthRedCapAll$RESPECT_HIV___0 = NULL
gpraAdult3monthRedCapAll$RESPECT_NONE___0 = NULL

colnames(gpraAdult3monthRedCapAll)[49:54] = c("DEPLOYEDIRAQ", "DEPLOYEDPERS", "DEPLOYEDASIA", "DEPLOYEDKOR", "DEPLOYEDWWII", "DEPLOYEDOTH")


colnames(gpraAdult3monthRedCapAll)[92:100] = c("RESPECT_RACE", "RESPECT_REL", "RESPECT_GENDER", "RESPECT_AGE", "RESPECT_SEXPR", "RESPECT_DISABLE", "RESPECT_MH", "RESPECT_HIV", "RESPECT_NONE")




gpraAdult3month = rbind(gpraAdult3month,gpraAdult3monthRedCapAll)

##############################################################
## Now get data for full people and put it into 3 month reassessment just get GPRA data
setwd("S:/Indiana Research & Evaluation/CCPE/CCPE SPSS - Datasets/3 Month Reassessments ADULT")

gpraAdultAllRedCapFull = read.csv("CCPEFollowUpAdultQueRedCapFull.csv", header= TRUE)
head(gpraAdultAllRedCapFull)
# Get rid of everything past svy_truth


gpraAdultAllRedCapFull = gpraAdultAllRedCapFull[,1:145]

names(gpraAdultAllRedCapFull) = toupper(names(gpraAdultAllRedCapFull))
head(gpraAdultAllRedCapFull)
# Just need the 1's for all the variables with 1's and zero's the ones have the data with one's and zero's




# Rename the one's
library(plyr)
library(tidyr)
head(gpraAdultAllRedCapFull)


gpraAdultAllRedCapFull$DEPLOYEDIRAQ___0 = NULL
gpraAdultAllRedCapFull$DEPLOYEDPERS___0 = NULL
gpraAdultAllRedCapFull$DEPLOYEDASIA___0 = NULL
gpraAdultAllRedCapFull$DEPLOYEDKOR___0 = NULL
gpraAdultAllRedCapFull$DEPLOYEDWWII___0 = NULL
gpraAdultAllRedCapFull$DEPLOYEDOTH___0 = NULL
gpraAdultAllRedCapFull$RESPECT_RACE___0 = NULL
gpraAdultAllRedCapFull$RESPECT_REL___0 = NULL
gpraAdultAllRedCapFull$RESPECT_GENDER___0 = NULL
gpraAdultAllRedCapFull$RESPECT_AGE___0 = NULL
gpraAdultAllRedCapFull$RESPECT_SEXPR___0 = NULL
gpraAdultAllRedCapFull$RESPECT_DISABLE___0 = NULL
gpraAdultAllRedCapFull$RESPECT_MH___0 = NULL
gpraAdultAllRedCapFull$RESPECT_HIV___0 = NULL
gpraAdultAllRedCapFull$RESPECT_NONE___0 = NULL

head(gpraAdultAllRedCapFull[,83:91])

colnames(gpraAdultAllRedCapFull[,83:91]) = c("RESPECT_RACE", "RESPECT_REL", "RESPECT_GENDER", "RESPECT_AGE", "RESPECT_SEXPR", "RESPECT_DISABLE", "RESPECT_MH", "RESPECT_HIV", "RESPECT_NONE")

colnames(gpraAdultAllRedCapFull[40:45]) = c("DEPLOYEDIRAQ", "DEPLOYEDPERS", "DEPLOYEDASIA", "DEPLOYEDKOR", "DEPLOYEDWWII", "DEPLOYEDOTH")




# Get rid of RECORD_ID, NATIONAL_MINORITY_SAHIV_PREVENTION_INITIATIVE_YOUT_TIMESTAMP, NAME, NATIONAL_MINORITY_SAHIV_PREVENTION_INITIATIVE_YOUT_COMPLETE
gpraAdultAllRedCapFull$RECORD_ID = NULL
gpraAdultAllRedCapFull$NAME = NULL
gpraAdultAllRedCapFull$NATIONAL_MINORITY_SAHIV_PREVENTION_INITIATIVE_YOUT_COMPLETE = NULL
gpraAdultAllRedCapFull$NATIONAL_MINORITY_SAHIV_PREVENTION_INITIATIVE_YOUT_TIMESTAMP = NULL
gpraAdultAllRedCapFull$NATIONAL_MINORITY_SAHIV_PREVENTION_INITIATIVE_ADUL_TIMESTAMP = NULL
# Now add columns with NA's for extra variables and make sure the NA's match the number of people in the redcap survey at the time so dim(name)[1]
# INSTRMNT_LANG	LANG_OTHER	GRANT_ID	DESIGNGRP	 MONTH	DAY	YEAR	INTTYPE	INTDUR	INTERVENTION_A	INTERVENTION_B	INTERVENTION_C
# Put all these variables together in another data set then cbind and put them in front
n = rep(NA, dim(gpraAdultAllRedCapFull)[1])
gpraAdultAllRedCapFull = cbind(INSTRMNT_LANG = n,LANG_OTHER = n, GRANT_ID = n, DESIGNGRP = n, PARTID= gpraAdultAllRedCapFull$REDCAP_SURVEY_IDENTIFIER, MONTH= n, DAY=n, YEAR = n, INTTYPE =n, INTDUR=n, INTERVENTION_A =n, INTERVENTION_B =n, INTERVENTION_C=n,gpraAdultAllRedCapFull)
head(gpraAdultAllRedCapFull)
gpraAdultAllRedCapFull$REDCAP_SURVEY_IDENTIFIER = NULL
write.csv(gpraAdult3month, "gpraAdult3month.csv", row.names = FALSE)
write.csv(gpraAdultAllRedCapFull, "gpraAdultAllRedCapFull.csv", row.names = FALSE)


gpraAdultAll = merge(gpraAdultBase, gpraAdult3month, by = "PARTID", all = TRUE)
head(gpraAdultAll)
setwd("C:/Users/Matthew.Hanauer/Desktop")
write.csv(gpraAdultAll, "gpraAdultAll.csv", row.names = FALSE)
dim(gpraAdultAll)




# Now Condom Scale ########## ########## ########## ########## ########## ########## ##########
condomScaleBase = read.spss("C:/Users/Matthew.Hanauer/Desktop/Matt'sData/Condom Scale - Baseline.sav", to.data.frame = TRUE)
condomScaleBase = rename(condomScaleBase, c("ID." = "PARTID"))

condomScale3month = read.spss("C:/Users/Matthew.Hanauer/Desktop/Matt'sData/Reassess 3M Condom Scale.sav", to.data.frame = TRUE)
condomScale3month = rename(condomScale3month, c("ID." = "PARTID"))

condomScale6month = read.spss("C:/Users/Matthew.Hanauer/Desktop/Matt'sData/Reassess 6M Condom Scale.sav", to.data.frame = TRUE)
condomScale6month = rename(condomScale6month, c("ID." = "PARTID"))

condomScaleAll = merge(condomScaleBase, condomScale3month, by = "PARTID", all = TRUE)
condomScaleAll = merge(condomScaleAll, condomScale3month, by = "PARTID", all = TRUE)
head(condomScaleAll)

# Now Pocket Screener ########## ########## ########## ########## ########## ########## ##########
pocketScreenerBase = read.spss("S:/Indiana Research & Evaluation/CCPE/CCPE SPSS - Datasets/Baseline Adult/SAMHSA Pocket Screen - Baseline.sav", to.data.frame = TRUE)
head(pocketScreenerBase)
#library(plyr)
#pocketScreenerBase = rename(pocketScreenerBase, c("ID" = "PARTID"))

pocketScreener3month = read.spss("S:/Indiana Research & Evaluation/CCPE/CCPE SPSS - Datasets/3 Month Reassessments ADULT/Reassess 3M SAMHSA Pocket Screen.sav", to.data.frame = TRUE)
#pocketScreener3month = rename(pocketScreener3month, c("ID" = "PARTID"))

#head(pocketScreener3month)

pocketScreenerAll = merge(pocketScreenerBase, pocketScreener3month, by = "ID", all = TRUE)
head(pocketScreenerAll)
dim(pocketScreenerAll)









