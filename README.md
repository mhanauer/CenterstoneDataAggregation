#### This data file loads data for both connections and CCPE. ######
library(foreign)
library(plyr)
setwd("C:/Users/Matthew.Hanauer/Desktop/Matt'sData")
# Here is the audit and I am breaking it down by baseline, 3m audit, and 6m audit.  Then I making sure the ID value is the same for each data set therefore I am using the rename function to change all of the ID names to PARTID.
auditBase = read.spss("C:/Users/Matthew.Hanauer/Desktop/Matt'sData/AUDIT - Baseline.sav", to.data.frame = TRUE)
auditBase = rename(auditBase, c("ID" = "PARTID"))

audit3Month = read.spss("C:/Users/Matthew.Hanauer/Desktop/Matt'sData/Reassess 3M AUDIT.sav", to.data.frame = TRUE)
audit3Month = rename(audit6Month, c("ID" = "PARTID"))

audit6Month = read.spss("C:/Users/Matthew.Hanauer/Desktop/Matt'sData/Reassess 6M AUDIT.sav", to.data.frame = TRUE)
audit6Month = rename(audit6Month, c("ID" = "PARTID"))

# I am first merging auditbase and audit3month, because I cannot figure out how to figure three at the same time.
# Then I am merging by all which is a full outer join meaning that IDs from all data set are included in the final data set.
auditAll = merge(auditBase, audit3Month, by = "PARTID", all = TRUE)
auditAll = merge(auditAll, audit6Month, by = "PARTID", all = TRUE)
head(auditAll)

## Same process as the audit for the GPRA
setwd("C:/Users/Matthew.Hanauer/Desktop/Matt'sData")
gpraAdultBase = read.csv("CCPE GRPA - Baseline_3.csv", header = TRUE)
gpraAdult3month = read.csv("Reassess 3M CCPE GPRA Adult.csv", header = TRUE)
gpraAdult6month = read.csv("Reassess 6M CCPE GPRA Adult.csv", header = TRUE) 
gpraAdultAll = merge(gpraAdultBase, gpraAdult3month, by = "PARTID", all = TRUE)
gpraAdultAll = merge(gpraAdultAll, gpraAdult6month, by = "PARTID", all = TRUE)
write.csv(gpraAdultAll, "gpraAdultAll.csv", row.names = FALSE)
# This is a test to prove that it is merging correctly.  1002 has both baseline and six month and their HIV answers are the same.
test = data.frame(subset(gpraAdultAll, PARTID == 1002))
write.csv(test, "test.csv", row.names = FALSE)

# Now GPRA Youth ########## ########## ########## ########## ########## ########## ##########
gpraYouthBase = read.csv("Baseline CCPE GPRA Youth.csv", header = TRUE)
gpraYouth3month = read.csv("3M CCPE GPRA Youth.csv", header = TRUE)
gpraYouth6month = read.csv("6M CCPE GPRA Youth.csv", header = TRUE)
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
pocketScreenerBase = read.spss("C:/Users/Matthew.Hanauer/Desktop/Matt'sData/SAMHSA Pocket Screen - Baseline.sav", to.data.frame = TRUE)
install.packages("plyr")
pocketScreenerBase = rename(pocketScreenerBase, c("ID" = "PARTID"))

pocketScreener3month = read.spss("C:/Users/Matthew.Hanauer/Desktop/Matt'sData/Reassess 3M SAMHSA Pocket Screen.sav", to.data.frame = TRUE)
pocketScreener3month = rename(pocketScreener3month, c("ID" = "PARTID"))

pocketScreener6month = read.spss("C:/Users/Matthew.Hanauer/Desktop/Matt'sData/Reassess 6M SAMHSA Pocket Screen.sav", to.data.frame = TRUE)
pocketScreener6month = rename(pocketScreener6month, c("ID" = "PARTID"))

pocketScreenerAll = merge(pocketScreenerBase, pocketScreener3month, by = "PARTID", all = TRUE)
pocketScreenerAll = merge(pocketScreenerAll, pocketScreener3month, by = "PARTID", all = TRUE)
head(pocketScreenerAll)

### Now start combining all of the data Merge the following data sets: auditAll, gpraAdultAll, gpraYouthAll, dastAll, hepCBase, condomScaleAll, pocketScreenerAll 
CCPEAlldat = merge(auditAll, gpraAdultAll, by = "PARTID", all = TRUE)  
CCPEAlldat = merge(CCPEAlldat, gpraYouthAll, by = "PARTID", all = TRUE)  
CCPEAlldat = merge(CCPEAlldat, dastAll, by = "PARTID", all = TRUE)  
CCPEAlldat = merge(CCPEAlldat, hepCBase, by = "PARTID", all = TRUE)  
CCPEAlldat = merge(CCPEAlldat, condomScaleAll, by = "PARTID", all = TRUE)
CCPEAlldat = merge(CCPEAlldat, pocketScreenerAll, by = "PARTID", all = TRUE)  
head(CCPEAlldat)
write.csv(CCPEAlldat, "CCPEAlldat.csv", row.names = FALSE)

#Connections #####  ##### ##### ##### ##### ##### ##### ##### ##### ##### ##### ##### #####
# Need the following data sets: Benefits Tracking, CSQ8 6 month, GAD7, GPRA, HCVRQ, HSUQ, PHQ9, SASSI3
## Benefits Tracking first
beneiftsTrackBase = read.spss("C:/Users/Matthew.Hanauer/Desktop/Matt'sDataConnections/Benefits Tracking/Benefits Tracking Baseline.sav", to.data.frame = TRUE)
beneiftsTrack3month = read.spss("C:/Users/Matthew.Hanauer/Desktop/Matt'sDataConnections/Benefits Tracking/Benefits Tracking Baseline.sav", to.data.frame = TRUE)
beneiftsTrack6month = read.spss("C:/Users/Matthew.Hanauer/Desktop/Matt'sDataConnections/Benefits Tracking/Benefits Tracking 6-month.sav", to.data.frame = TRUE)


benefitsAll = merge(beneiftsTrackBase, beneiftsTrack3month, by = "ParticipantID", all = TRUE)
benefitsAll = merge(benefitsAll, beneiftsTrack6month, by = "ParticipantID", all = TRUE)

### CSQ8 #####  ##### ##### ##### ##### ##### ##### ##### ##### ##### ##### ##### #####
CSQ86month = read.spss("C:/Users/Matthew.Hanauer/Desktop/Matt'sDataConnections/CSQ8/CSQ8 6 Month.sav", to.data.frame = TRUE)

### GAD7 #####  ##### ##### ##### ##### ##### ##### ##### ##### ##### ##### ##### #####
# Remember that this data is 6 and 12 month assessments not 3 and 6 month
setwd("C:/Users/Matthew.Hanauer/Desktop/Matt'sDataConnections/GAD7")
GAD7Base = read.spss("C:/Users/Matthew.Hanauer/Desktop/Matt'sDataConnections/GAD7/GAD7 Baseline.sav", to.data.frame = TRUE)
GAD76month = read.spss("C:/Users/Matthew.Hanauer/Desktop/Matt'sDataConnections/GAD7/GAD7 6 Month.sav", to.data.frame = TRUE)
GAD712month = read.spss("C:/Users/Matthew.Hanauer/Desktop/Matt'sDataConnections/GAD7/GAD7 12 Month.sav", to.data.frame = TRUE)

GAD7All = merge(GAD7Base, GAD76month, by = "ParticipantID", all = TRUE)
GAD7All = merge(GAD7All, GAD712month, by = "ParticipantID", all = TRUE)

### GPRA #####  ##### ##### ##### ##### ##### ##### ##### ##### ##### ##### ##### #####
setwd("C:/Users/Matthew.Hanauer/Desktop/Matt'sDataConnections/GPRA")
GPRABase = read.spss("C:/Users/Matthew.Hanauer/Desktop/Matt'sDataConnections/Benefits Tracking/Benefits Tracking Baseline.sav", to.data.frame = TRUE)
GPRA6month = read.spss("C:/Users/Matthew.Hanauer/Desktop/Matt'sDataConnections/Benefits Tracking/Benefits Tracking 6-month.sav", to.data.frame = TRUE)
GPRAAll = merge(GPRABase, GPRA6month, by = "ParticipantID", all = TRUE)

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
setwd("C:/Users/Matthew.Hanauer/Desktop/Matt'sDataConnections/PHQ9  ")
PHQ9Base = read.spss("C:/Users/Matthew.Hanauer/Desktop/Matt'sDataConnections/PHQ9/PHQ9 Baseline.sav", to.data.frame = TRUE)
PHQ96month = read.spss("C:/Users/Matthew.Hanauer/Desktop/Matt'sDataConnections/PHQ9/PHQ9 6 Month.sav", to.data.frame = TRUE)
PHQ912month = read.spss("C:/Users/Matthew.Hanauer/Desktop/Matt'sDataConnections/PHQ9/PHQ9 12 Month.sav", to.data.frame = TRUE)
PHQ9All = merge(PHQ9Base, PHQ96month, by = "ParticipantID", all = TRUE)
PHQ9All = merge(PHQ9All, PHQ912month, by = "ParticipantID", all = TRUE)

### SASSI3 #####  ##### ##### ##### ##### ##### ##### ##### ##### ##### ##### ##### #####
setwd("C:/Users/Matthew.Hanauer/Desktop/Matt'sDataConnections/SASSI3")
SASSI3Base = read.spss("C:/Users/Matthew.Hanauer/Desktop/Matt'sDataConnections/SASSI3/SASSI3 Baseline.sav", to.data.frame = TRUE)
SASSI36month = read.spss("C:/Users/Matthew.Hanauer/Desktop/Matt'sDataConnections/SASSI3/SASSI3 6 Months.sav", to.data.frame = TRUE)
SASSI3All = merge(SASSI3Base, SASSI36month, by = "ParticipantID", all = TRUE)



