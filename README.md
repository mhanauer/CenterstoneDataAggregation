library(foreign)
library(plyr)
setwd("C:/Users/Matthew.Hanauer/Desktop/Matt'sData")
install.packages("rprojroot")
install.packages("devtools")
devtools::install_github("rstudio/rmarkdown")
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
gpraAdultBase = read.spss("C:/Users/Matthew.Hanauer/Desktop/Matt'sData/CCPE GRPA - Baseline_3.sav", to.data.frame = TRUE)

gpraAdult3month = read.spss("C:/Users/Matthew.Hanauer/Desktop/Matt'sData/Reassess 3M CCPE GPRA Adult.sav", to.data.frame = TRUE)


gpraAdult6month = read.spss("C:/Users/Matthew.Hanauer/Desktop/Matt'sData/Reassess 6M CCPE GPRA Adult.sav", to.data.frame = TRUE)

gpraAdultAll = merge(gpraAdultBase, gpraAdult3month, by = "PARTID", all = TRUE)
gpraAdultAll = merge(gpraAdultAll, gpraAdult6month, by = "PARTID", all = TRUE)



# Now GPRA Youth ########## ########## ########## ########## ########## ########## ##########
gpraYouthBase = read.spss("C:/Users/Matthew.Hanauer/Desktop/Matt'sData/Baseline CCPE GPRA Youth.sav", to.data.frame = TRUE)
gpraYouth3month = read.spss("C:/Users/Matthew.Hanauer/Desktop/Matt'sData/Reassess 3M CCPE GPRA Youth.sav", to.data.frame = TRUE)
grpaYouth6month = read.spss("C:/Users/Matthew.Hanauer/Desktop/Matt'sData/Reassess 6M CCPE GPRA Youth.sav", to.data.frame = TRUE)
grpaYouthAll = merge(gpraYouthBase, gpraYouth3month, by = "PARTID", all = TRUE)
grpaYouthAll = merge(grpaYouthAll, gpraYouth3month, by = "PARTID", all = TRUE)
head(grpaYouthAll)

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
pocketScreenerBase = rename(pocketScreenerBase, c("ID" = "PARTID"))

pocketScreener3month = read.spss("C:/Users/Matthew.Hanauer/Desktop/Matt'sData/Reassess 3M SAMHSA Pocket Screen.sav", to.data.frame = TRUE)
pocketScreener3month = rename(pocketScreener3month, c("ID" = "PARTID"))

pocketScreener6month = read.spss("C:/Users/Matthew.Hanauer/Desktop/Matt'sData/Reassess 6M SAMHSA Pocket Screen.sav", to.data.frame = TRUE)
pocketScreener6month = rename(pocketScreener6month, c("ID" = "PARTID"))

pocketScreenerAll = merge(pocketScreenerBase, pocketScreener3month, by = "PARTID", all = TRUE)
pocketScreenerAll = merge(pocketScreenerAll, pocketScreener3month, by = "PARTID", all = TRUE)
head(pocketScreenerAll)

### Now start combining all of the data Merge the following data sets: auditAll, gpraAdultAll, grpaYouthAll, dastAll, hepCBase, condomScaleAll, pocketScreenerAll 
CCPEAlldat = merge(auditAll, gpraAdultAll, by = "PARTID", all = TRUE)  
CCPEAlldat = merge(CCPEAlldat, grpaYouthAll, by = "PARTID", all = TRUE)  
CCPEAlldat = merge(CCPEAlldat, dastAll, by = "PARTID", all = TRUE)  
CCPEAlldat = merge(CCPEAlldat, hepCBase, by = "PARTID", all = TRUE)  
CCPEAlldat = merge(CCPEAlldat, condomScaleAll, by = "PARTID", all = TRUE)
CCPEAlldat = merge(CCPEAlldat, pocketScreenerAll, by = "PARTID", all = TRUE)  
head(CCPEAlldat)
write.csv(CCPEAlldat, "CCPEAlldat.csv", row.names = FALSE)
