# tradingviwe
# trend line
// This source code is subject to the terms of the Mozilla Public License 2.0 at https://mozilla.org/MPL/2.0/
// © mohamadmansouri3295

//@version=5
indicator("trendline & pivote" , 'trendline & pp' , true ,  max_lines_count = 500 )
period =  input.int(8 , minval = 6)
pivotwidth = input.int (1)
showPP = input.bool( false,"Show Pivot Points Labels")
ShowFB = input.bool(true, "Show Fractal Break Arrows")
showpin = input.bool(true , 'show green pinbar' )
colres = input.color (color.blue )
colsup = input.color (color.white)
// ////

pivoth = ta.pivothigh(period , period)
pivotl = ta.pivotlow (period , period)

ph = ta.valuewhen (pivoth , bar_index , 0)
pl = ta.valuewhen (pivotl , bar_index , 0)


////hh line up

h_x1= ta.valuewhen(pivoth , bar_index[period] ,1 )
h_x2= ta.valuewhen(pivoth , bar_index[period] ,0 )
h_y1 = ta.valuewhen(pivoth, high[period], 1)
h_y2 = ta.valuewhen(pivoth, high[period], 0)
h_y_1 = ta.valuewhen(pivoth, high[period], 1)
h_y_2 = ta.valuewhen(pivoth, high[period], 0)

higherhigh = na(pivoth) ? na : h_y1 <  h_y2 ? pivoth : na
lowerhigh = na(pivoth) ? na : h_y_1 >  h_y_2 ? pivoth : na
//   lineup 2
x1 = ta.valuewhen(pivoth , bar_index[period] ,2 )
x2 = ta.valuewhen(pivoth , bar_index[period] ,1 )
y1 = ta.valuewhen(pivoth, high[period], 2)
y2 = ta.valuewhen(pivoth, high[period], 1)

h_x_1= ta.valuewhen(pivoth , bar_index[period] ,3 )
h_x_2= ta.valuewhen(pivoth , bar_index[period] ,2 )
h_y11 = ta.valuewhen(pivoth, high[period], 3)
h_y12 = ta.valuewhen(pivoth, high[period], 2)

///////ll line down 

l_x1 = ta.valuewhen(pivotl , bar_index[period] ,1 )
l_x2 = ta.valuewhen(pivotl , bar_index[period] ,0 ) 
l_y1 = ta.valuewhen(pivotl , low[period] ,1 )
l_y2 = ta.valuewhen(pivotl , low[period] ,0 )
l_y_1 = ta.valuewhen(pivotl, low[period], 1)
l_y_2 = ta.valuewhen(pivotl, low[period ], 0)
lowerlow = na(pivotl) ? na : l_y_1 > l_y_2 ? pivotl : na
higherlow = na(pivotl) ? na : l_y_1 < l_y_2 ? pivotl : na

// ///// line down 2
x1l = ta.valuewhen(pivotl , bar_index[period] ,2 )
x2l = ta.valuewhen(pivotl , bar_index[period] ,1 ) 
y1l = ta.valuewhen(pivotl , low[period] ,2 )
y2l = ta.valuewhen(pivotl , low[period] ,1 )

l_x11 = ta.valuewhen(pivotl , bar_index[period] ,3 )
l_x12 = ta.valuewhen(pivotl , bar_index[period] ,2 ) 
l_y11 = ta.valuewhen(pivotl , low[period] ,3 )
l_y12 = ta.valuewhen(pivotl , low[period] ,2 )

plotshape(showPP ? higherhigh  : na, title='HH',  location=location.abovebar, color=color.new(color.green,50), text="HH",textcolor=color.new(color.green,50),offset = -period)
plotshape(showPP ? lowerhigh  : na, title='LH',  location=location.abovebar, color=color.new(color.red,50), text="LH",textcolor=color.new(color.red,50),offset = -period)
plotshape(showPP ? higherlow  : na, title='HL',  location=location.belowbar, color=color.new(color.green,50), text="HL",textcolor=color.new(color.green,50),offset = -period)
plotshape(showPP ? lowerlow  : na, title='LL', location=location.belowbar, color=color.new(color.red,50), text="LL",textcolor=color.new(color.red,50),offset = -period)

// //  Fractal Break Alerts
pvthis = 0.0
pvtlos = 0.0
pvthis := na(pivoth) ? pvthis[1] : high[period]
pvtlos := na(pivotl) ? pvtlos[1] : low[ period]

bull = false
bear = false
bull  := close>pvthis and open<=pvthis 
bear := close<pvtlos and open>=pvtlos 


plotshape(ShowFB and bull?1:na, title="BUY Arrow", style= shape.arrowup ,location = location.belowbar , size=size.normal, color=color.green)
plotshape(ShowFB and bear?-1:na, title="SELL Arrow", style= shape.arrowdown ,location = location.abovebar  , size=size.normal, color=color.red)


/////////////////////////////////

if bull
    alert(" CHART مقاومت شکست", alert.freq_once_per_bar)
if bear
    alert("پشتیبانی شکست CHART", alert.freq_once_per_bar)    

   
    
 //////////
lineup = line.new(h_x1 ,h_y1 ,h_x2 ,h_y2 ,xloc.bar_index , extend.right ,color= color.new(colres ,10 ) ,width = 3)
line.delete (lineup [1])
lineup1 = line.new( x1 ,y1 ,x2 ,y2 ,xloc.bar_index , extend.right ,color= color.new(colres , 30 ) ,width = 2)
line.delete (lineup1 [1])
lineup2 = line.new( h_x_1 ,h_y11 ,h_x_2 ,h_y12 ,xloc.bar_index , extend.right ,color= color.new(colres , 30 ), width = 2)
line.delete (lineup2 [1])

linedn = line.new (l_x1 ,l_y1 , l_x2 , l_y2 ,xloc.bar_index , extend.right ,color= color.new(colsup , 10 ) , width = 3 )
line.delete(linedn [1])
linedn1 = line.new (x1l ,y1l , x2l , y2l,xloc.bar_index , extend.right ,color= color.new(colsup , 30 ) , width = 2)
line.delete(linedn1 [1])
linedn2 = line.new (l_x11 ,l_y11 , l_x12 , l_y12 ,xloc.bar_index , extend.right ,color= color.new(colsup , 30 ), width = 2)
line.delete(linedn2 [1])







