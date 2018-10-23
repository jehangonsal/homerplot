# homerplot
This is a silly plot that features a happy Homer Simpson on top

### Let's create a fun visualisation with our dear friend Homer Simpson

library(tidyverse)
library(rvest)
library(httr)
library(jpeg)
library(magick)

Area <- row.names(datasets::USPersonalExpenditure)

homer <- image_read("http://pngimg.com/uploads/simpsons/simpsons_PNG88.png")

fig <- image_graph(width = 700, height = 600, res = 96)
datasets::USPersonalExpenditure %>% as.data.frame() %>% cbind(Area) %>% gather(key = Year, value = Spend, -Area) %>%
  mutate(Spend = Spend*1000000000) %>%
  ggplot(aes(x = Year, y = Spend, group = Area)) + geom_line(aes(color = Area), size = .8, alpha = .8) + theme_bw() +
  labs(title = "Spending is up this year!", subtitle = "Good news for the economy!", 
       caption = "Unfortunately, they're buying cigarettes") + 
  scale_y_continuous(labels = scales::dollar)
dev.off()

out <- image_composite(fig, image_scale(homer, "300"), offset = "+50+120")
print(out)


