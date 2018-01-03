library(foreign)
library(h2o)
# Get R markdown working later
# Get audit in first and combine the rest after.  Can be fine, because you will do a full join.
auditBase = read.spss("C:/Users/Matthew.Hanauer/Desktop/Matt'sData/AUDIT - Baseline.sav", to.data.frame = TRUE)
auditBase = rename(auditBase, c("ID" = "PARTID"))

audit3Month = read.spss("C:/Users/Matthew.Hanauer/Desktop/Matt'sData/Reassess 3M AUDIT.sav", to.data.frame = TRUE)
audit3Month = rename(audit6Month, c("ID" = "PARTID"))

audit6Month = read.spss("C:/Users/Matthew.Hanauer/Desktop/Matt'sData/Reassess 6M AUDIT.sav", to.data.frame = TRUE)
audit6Month = rename(audit6Month, c("ID" = "PARTID"))

auditAll = merge(auditBase, audit3Month, by = "PARTID", all = TRUE)
auditAll = merge(auditAll, audit6Month, by = "PARTID", all = TRUE)
head(auditAll)

# Now GPRA Adult.  Need to rename the PARTID to ID.  Make sure you aggregate baseline with 3 month then with 6 month
gpraAdultBase = read.spss("S:/Indiana Research & Evaluation/CCPE/CCPE SPSS - Datasets/Baseline Adult/CCPE GRPA - Baseline.sav", use.value.labels = FALSE, to.data.frame = TRUE)
gpraAdult3month = read.spss("S:/Indiana Research & Evaluation/CCPE/CCPE SPSS - Datasets/3 Month Reassessments ADULT/Reassess 3M CCPE GPRA Adult.sav", use.value.labels = FALSE, to.data.frame = TRUE)
## Need to add the 3 month from the Red for the only GRPA and all assessments, but just drop all other assessments so everything after svytruth
setwd("S:/Indiana Research & Evaluation/CCPE/CCPE SPSS - Datasets/3 Month Reassessments ADULT")
gpraAdult3monthRedCap = read.csv("CCPEFollowupAdultQueRedCap.csv", header = TRUE)
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

YEAR = data.frame(apply(YEAR, 2, function(x){ifelse(x > 12/13/2016, 2017, ifelse(x > 12/31/2017, 2018, x))}))
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
DAY = n
YEAR = YEAR
INTTYPE =n
INTDUR = n
INTERVENTION_A =n 
INTERVENTION_B = n
INTERVENTION_C = n
gpraAdult3monthRedCapFirst = cbind(INSTRMNT_LANG, LANG_OTHER, GRANT_ID, DESIGNGRP, PARTID = gpraAdult3monthRedCapAll$REDCAP_SURVEY_IDENTIFIER, MONTH, DAY, YEAR, INTTYPE, INTDUR, INTERVENTION_A, INTERVENTION_B, INTERVENTION_C)
gpraAdult3monthRedCapAll$REDCAP_SURVEY_IDENTIFIER = NULL
gpraAdult3monthRedCapAll = cbind(gpraAdult3monthRedCapFirst, gpraAdult3monthRedCapAll)
summary(gpraAdult3monthRedCapAll)


# Now combine all the new variables in front of previous GPRA so it is the same order as before.  Should just rbind them to the 3 month reassessment.
# Double check duplicate IDs. 
colnames(gpraAdult3monthRedCapAll)[49:60] = c("DEPLOYEDIRAQ", "Wrong1", "DEPLOYEDPERS", "Wrong2", "DEPLOYEDASIA", "Wrong3", "DEPLOYEDKOR", "Wrong4", "DEPLOYEDWWII", "Wrong5", "DEPLOYEDOTH", "Wrong6")

gpraAdult3monthRedCapAll$Wrong0 = NULL
gpraAdult3monthRedCapAll$Wrong1 = NULL
gpraAdult3monthRedCapAll$Wrong2 = NULL
gpraAdult3monthRedCapAll$Wrong3 = NULL
gpraAdult3monthRedCapAll$Wrong4 = NULL
gpraAdult3monthRedCapAll$Wrong5 = NULL
gpraAdult3monthRedCapAll$Wrong6 = NULL

colnames(gpraAdult3monthRedCapAll)[92:109] = c("RESPECT_RACE", "Wrong1", "RESPECT_REL", "Wrong2", "RESPECT_GENDER", "Wrong3", "RESPECT_AGE", "Wrong4", "RESPECT_SEXPR", "Wrong5", "RESPECT_DISABLE", "Wrong6", "RESPECT_MH", "Wrong7", "RESPECT_HIV", "Wrong8", "RESPECT_NONE", "Wrong9")
gpraAdult3monthRedCapAll$Wrong1 = NULL
gpraAdult3monthRedCapAll$Wrong2 = NULL
gpraAdult3monthRedCapAll$Wrong3 = NULL
gpraAdult3monthRedCapAll$Wrong4 = NULL
gpraAdult3monthRedCapAll$Wrong5 = NULL
gpraAdult3monthRedCapAll$Wrong6 = NULL
gpraAdult3monthRedCapAll$Wrong7 = NULL
gpraAdult3monthRedCapAll$Wrong8 = NULL
gpraAdult3monthRedCapAll$Wrong9 = NULL



gpraAdult3monthTest = rbind(gpraAdult3month,gpraAdult3monthRedCapAll)
head(gpraAdult3monthTest)
gpraAdult6month = read.spss("S:/Indiana Research & Evaluation/CCPE/CCPE SPSS - Datasets/6 Month Reassessments ADULT/Reassess 6M CCPE GPRA Adult.sav", use.value.labels = FALSE, to.data.frame = TRUE)
gpraAdultAll = merge(gpraAdultBase, gpraAdult3month, by = "PARTID", all = TRUE)
gpraAdultAll = merge(gpraAdultAll, gpraAdult6month, by = "PARTID", all = TRUE)
# This is a test to prove that it is merging correctly.  1002 has both baseline and six month and their HIV answers are the same.
test = data.frame(subset(gpraAdultAll, PARTID == 1002))
write.csv(test, "test.csv", row.names = FALSE)
# Now GPRA Youth ########## ########## ########## ########## ########## ########## ##########
gpraYouthBase = read.spss("S:/Indiana Research & Evaluation/CCPE/CCPE SPSS - Datasets/YOUTH Datasets/Baseline CCPE GPRA Youth.sav", use.value.labels = FALSE, to.data.frame = TRUE)
gpraYouth3month = read.spss("S:/Indiana Research & Evaluation/CCPE/CCPE SPSS - Datasets/YOUTH Datasets/Reassess 3M CCPE GPRA Youth.sav", use.value.labels = FALSE, to.data.frame = TRUE)
gpraYouth6month = read.spss("S:/Indiana Research & Evaluation/CCPE/CCPE SPSS - Datasets/YOUTH Datasets/Reassess 6M CCPE GPRA Youth.sav", use.value.labels = FALSE, to.data.frame = TRUE)

gpraYouthAll = merge(gpraYouthBase, gpraYouth3month, by = "PARTID", all = TRUE)
gpraYouthAll = merge(gpraYouthAll, gpraYouth3month, by = "PARTID", all = TRUE)
gpraYouthAll = data.frame(gpraYouthAll)
head(gpraYouthAll)



# Now DAST  ########## ########## ########## ########## ########## ########## ##########
dastBase = read.spss("C:/Users/Matthew.Hanauer/Desktop/Matt'sData/DAST - Baseline.sav", to.data.frame = TRUE)
dastBase = rename(dastBase, c("ID" = "PARTID"))

dast3month = read.spss("C:/Users/Matthew.Hanauer/Desktop/Matt'sData/Reassess 3M DAST.sav", to.data.frame = TRUE)
dast3month = rename(dast3month, c("ID" = "PARTID"))

dast6month = read.spss("C:/Users/Matthew.Hanauer/Desktop/Matt'sData/Reassess 6M DAST.sav", to.data.frame = TRUE)
dast6month = rename(dast6month, c("ID" = "PARTID"))

dastAll = merge(dastBase, dast3month, by = "PARTID", all = TRUE)
dastAll = merge(dastAll, dast3month, by = "PARTID", all = TRUE)
head(dastAll)

# Now HepC ########## ########## ########## ########## ########## ########## ##########
hepCBase = read.spss("C:/Users/Matthew.Hanauer/Desktop/Matt'sData/HepC Risk Questionnaire - Baseline.sav", to.data.frame = TRUE)
hepCBase = rename(hepCBase, c("ID" = "PARTID"))

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
library(plyr)
pocketScreenerBase = rename(pocketScreenerBase, c("ID" = "PARTID"))

pocketScreener3month = read.spss("S:/Indiana Research & Evaluation/CCPE/CCPE SPSS - Datasets/3 Month Reassessments ADULT/Reassess 3M SAMHSA Pocket Screen.sav", to.data.frame = TRUE)
pocketScreener3month = rename(pocketScreener3month, c("ID" = "PARTID"))


pocketScreenerAll = merge(pocketScreenerBase, pocketScreener3month, by = "PARTID", all = TRUE)
head(pocketScreenerAll)



#Connections #####  ##### ##### ##### ##### ##### ##### ##### ##### ##### ##### ##### #####
# Need the following data sets: Benefits Tracking, CSQ8 6 month, GAD7, GPRA, HCVRQ, HSUQ, PHQ9, SASSI3
## Benefits Tracking first
beneiftsTrackBase = read.spss("S:/Indiana Research & Evaluation/Indiana Connections/Data/Benefits Tracking/Benefits Tracking Baseline.sav",use.value.labels = FALSE, to.data.frame = TRUE)
beneiftsTrack6month = read.spss("S:/Indiana Research & Evaluation/Indiana Connections/Data/Benefits Tracking/Benefits Tracking 6-month.sav",use.value.labels = FALSE, to.data.frame = TRUE)


benefitsAll = merge(beneiftsTrackBase, beneiftsTrack6month, by = "ParticipantID", all = TRUE)

### CSQ8 #####  ##### ##### ##### ##### ##### ##### ##### ##### ##### ##### ##### #####
CSQ86month = read.spss("C:/Users/Matthew.Hanauer/Desktop/Matt'sDataConnections/CSQ8/CSQ8 6 Month.sav", to.data.frame = TRUE)

### GAD7 #####  ##### ##### ##### ##### ##### ##### ##### ##### ##### ##### ##### #####
# Remember that this data is 6 and 12 month assessments not 3 and 6 month
GAD7Base = read.spss("S:/Indiana Research & Evaluation/Indiana Connections/Data/GAD7/GAD7 Baseline.sav", to.data.frame = TRUE)
GAD76month = read.spss("S:/Indiana Research & Evaluation/Indiana Connections/Data/GAD7/GAD7 6 Month.sav", to.data.frame = TRUE)
GAD712month = read.spss("S:/Indiana Research & Evaluation/Indiana Connections/Data/GAD7/GAD7 12 Month.sav", to.data.frame = TRUE)

GAD7All = merge(GAD7Base, GAD76month, by = "ParticipantID", all = TRUE)
GAD7All = merge(GAD7All, GAD712month, by = "ParticipantID", all = TRUE)


### HCVRQ #####  ##### ##### ##### ##### ##### ##### ##### ##### ##### ##### ##### #####
setwd("C:/Users/Matthew.Hanauer/Desktop/Matt'sDataConnections/HCVRQ")

HCVRQBase = read.spss("C:/Users/Matthew.Hanauer/Desktop/Matt'sDataConnections/HCVRQ/HCVRQ Baseline.sav", to.data.frame = TRUE)
HCVRQ6month = read.spss("C:/Users/Matthew.Hanauer/Desktop/Matt'sDataConnections/HCVRQ/HCVRQ 6 Month.sav", to.data.frame = TRUE)
HCVRQAll = merge(HCVRQBase, HCVRQ6month, by = "ParticipantID", all = TRUE)

### HSUQ #####  ##### ##### ##### ##### ##### ##### ##### ##### ##### ##### ##### #####
setwd("C:/Users/Matthew.Hanauer/Desktop/Matt'sDataConnections/HSUQ")
HSUQBase = read.spss("C:/Users/Matthew.Hanauer/Desktop/Matt'sDataConnections/HSUQ/HSUQ Baseline.sav", to.data.frame = TRUE)
HSUQ6month = read.spss("C:/Users/Matthew.Hanauer/Desktop/Matt'sDataConnections/HSUQ/HSUQ 6 Month.sav", to.data.frame = TRUE)
HSUQAll = merge(HSUQBase, HSUQ6month, by = "ParticipantID", all = TRUE)

### PHQ9 #####  ##### ##### ##### ##### ##### ##### ##### ##### ##### ##### ##### #####
setwd("C:/Users/Matthew.Hanauer/Desktop/Matt'sDataConnections/PHQ9")
PHQ9Base = read.spss("S:/Indiana Research & Evaluation/Indiana Connections/Data/PHQ9/PHQ9 Baseline.sav", to.data.frame = TRUE)
PHQ96month = read.spss("S:/Indiana Research & Evaluation/Indiana Connections/Data/PHQ9/PHQ9 6 Month.sav", to.data.frame = TRUE)
PHQ912month = read.spss("S:/Indiana Research & Evaluation/Indiana Connections/Data/PHQ9/PHQ9 12 Month.sav", to.data.frame = TRUE)
PHQ9All = merge(PHQ9Base, PHQ96month, by = "ParticipantID", all = TRUE)
PHQ9All = merge(PHQ9All, PHQ912month, by = "ParticipantID", all = TRUE)

### SASSI3 #####  ##### ##### ##### ##### ##### ##### ##### ##### ##### ##### ##### #####
setwd("C:/Users/Matthew.Hanauer/Desktop/Matt'sDataConnections/SASSI3")
SASSI3Base = read.spss("C:/Users/Matthew.Hanauer/Desktop/Matt'sDataConnections/SASSI3/SASSI3 Baseline.sav", to.data.frame = TRUE)
SASSI36month = read.spss("C:/Users/Matthew.Hanauer/Desktop/Matt'sDataConnections/SASSI3/SASSI3 6 Months.sav", to.data.frame = TRUE)
SASSI3All = merge(SASSI3Base, SASSI36month, by = "ParticipantID", all = TRUE)

#### GRPA ######## ########################################################################
#GPRAConBase =read.spss("S:/Indiana Research & Evaluation/Indiana Connections/Data/GPRA/GPRA Baseline.sav", use.value.labels = FALSE, to.data.frame = TRUE) 
#GPRAConMonth6 =read.spss("S:/Indiana Research & Evaluation/Indiana Connections/Data/GPRA/GPRA 6 Months.sav", use.value.labels = FALSE, to.data.frame = TRUE) 
#GPRAAll = merge(GPRAConBase, GPRAConMonth6, by = "ParticipantID", all = TRUE)
setwd("S:/Indiana Research & Evaluation/Indiana Connections/Data/GPRA")
GPRAConBase = read.csv("GPRA Baseline.csv", header = TRUE) 
GPRAConMonth6 = read.csv("GPRA 6 Months.csv", header = TRUE)
colnames(GPRAConBase)[1]= "ParticipantID"  
colnames(GPRAConMonth6)[1]= "ParticipantID"  
GPRAAll = merge(GPRAConBase, GPRAConMonth6, by = "ParticipantID", all = TRUE)

