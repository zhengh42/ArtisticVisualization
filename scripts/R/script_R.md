
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
