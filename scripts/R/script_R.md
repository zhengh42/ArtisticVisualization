
### Fireworks

```
### Naive version (July 4th, 2019)
library(ggplot2)
library(grid)
library(gridExtra)
library(RColorBrewer)

# Define x and y according to projectile motion function (neglecting air resistance)
t=seq(0,150,1) # time
g=9.8 # gravity constant
v=seq(800,1500,1) # velocity

myCols=c("gold","firebrick2","royalblue1","seagreen4","purple1","sienna1")

OneBlast = function(x0,y0){
  myfireworks=NULL
  blastcol=sample(myCols,1)
  for(theta in seq(0,360,2)){
    v0=sample(v,1)
    x=v0*t*cos(theta*pi/180)
    y=v0*t*sin(theta*pi/180)-0.5*g*t^2
    firework=data.frame(x=x0+x,y=y0+y,time=t,theta=theta,blastcol=blastcol)
    myfireworks=rbind(myfireworks,firework)
  }
  return(myfireworks)
}

# Creat blasts
  myfireworks=NULL
  n=17
  x=runif(n, min=-500000, max=500000)
  y=runif(n, min=-500000, max=500000)
  for(i in 1:n){
    firework=OneBlast(x[i],y[i])
    firework$theta=paste0(i,".",firework$theta)
    myfireworks=rbind(myfireworks,firework)
  }

# Plot
bgcol="black"
themenull=theme(legend.position="none",plot.background = element_rect(fill = bgcol,colour = bgcol),panel.background = element_rect(fill=bgcol,colour = bgcol),panel.grid = element_blank(),axis.ticks=element_blank(),axis.title=element_blank(),axis.text =element_blank(),plot.margin=unit(c(0,0,0,0), "pt"))

p=ggplot(myfireworks) + geom_path(aes(x = x, y = y, group=theta, colour = blastcol,alpha=0.3),size=0.2) + scale_colour_identity() + themenull

png(paste0("/Users/zhengh42/Dropbox/projects/ArtisticVisualization/output/","fireworks_190704.png"),width = 3200,height = 1600,units="px",res=600)
grid.draw(grobTree(rectGrob(gp=gpar(fill=bgcol,lwd=0)),grid.arrange(grobs = list(p))))
dev.off()
```

### Tuberculosis burden in China

```
require(data.table)
require(ggplot2)
data=fread("/Users/zhengh42/Dropbox/projects/ArtisticVisualization/input/IHME-GBD_2017_DATA-TB-China.csv")
data.subset=data[age_name=="All Ages" & (measure_name=="Incidence" | measure_name=="Prevalence") & metric_name!="Percent"]

translator <- c(
  Incidence = "Incidence(发病)",
  Prevalence = "Prevalence(患病)",
  Number = "Number(总数)",
  Percent = "Percent(百分比)",
  Rate = "Rate(率)"
)

png("/Users/zhengh42/Dropbox/projects/ArtisticVisualization/output/GBD_TB_China.png",width = 2000,height = 1500,units="px",res=300)

ggplot(data.subset,aes(x=year,y=val,color=factor(sex_name,labels=c("Both"="Both(总数)","Female"="Female(女)","Male"="Male(男)")))) +
  geom_line(alpha=0.5) +
  scale_x_continuous(breaks=seq(1990,2017,5)) +
  ggtitle("1990-2017年中国肺结核发病和患病情况") + labs(y="",x="year(年份)") +
  facet_wrap(measure_name ~ metric_name ,scales="free",labeller = labeller(measure_name = translator,metric_name=translator)) +
  theme(text=element_text(size=10,family='STKaiti'),panel.border = element_blank(),panel.grid.major = element_blank(),panel.grid.minor = element_blank(),legend.position = "right",legend.title = element_blank()) +
  scale_color_manual(values=c("Both(总数)"="black","Female(女)"="#fc8d62","Male(男)"="#66c2a5"))

dev.off()
```
