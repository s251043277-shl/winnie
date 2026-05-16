# Filter the data for Machine 1, Temperature 303, and Pressure 100
machine1_temp303_pressure100 <- X031[X031$Machine == 1 & X031$Temperature == 303 & X031$Pressure == 100, ]

# Ensure PartLength is numeric
machine1_temp303_pressure100$PartLength <- as.numeric(machine1_temp303_pressure100$PartLength)

# Create the xbar.one chart for PartLength
xbar_chart <- qcc(machine1_temp303_pressure100$PartLength, type = "xbar.one", plot = FALSE)

# Convert qcc object to a data frame for plotting with plotly
plot_data <- data.frame(
  x = 1:length(xbar_chart$data),
  y = xbar_chart$data,
  lcl = xbar_chart$limits[1],
  ucl = xbar_chart$limits[2],
  cl = xbar_chart$center
)

# Plotting with Plotly
p <- plot_ly(plot_data, x = ~x, y = ~y, type = 'scatter', mode = 'lines+markers', name = 'PartLength') %>%
  add_trace(y = ~lcl, mode = 'lines', name = 'LCL', line = list(color = '#0072B2', dash = 'dash')) %>%
  add_trace(y = ~ucl, mode = 'lines', name = 'UCL', line = list(color = '#D55E00', dash = 'dash')) %>%
  add_trace(y = ~cl, mode = 'lines', name = 'CL', line = list(color = '#009E73')) %>%
  layout(title = list(text = 'X-bar Control Chart for Part Length (Machine 1, Temp 303, Press 100)', font = list(size = 18)),
         xaxis = list(title = list(text = 'Observation', font = list(size = 18)), tickfont = list(size = 14), zeroline = FALSE, showgrid = FALSE),
         yaxis = list(title = list(text = 'Part Length', font = list(size = 18)), tickfont = list(size = 14), zeroline = FALSE, showgrid = FALSE, rangemode = "tozero"),
         plot_bgcolor = 'white', paper_bgcolor = 'white',
         annotations = list(
            list(x = 0.98, y = xbar_chart$limits[1], text = paste('LCL:', round(xbar_chart$limits[1], 2)), showarrow = FALSE, xanchor = 'right', yanchor = 'middle', font = list(color = '#0072B2', size = 12)),
            list(x = 0.98, y = xbar_chart$limits[2], text = paste('UCL:', round(xbar_chart$limits[2], 2)), showarrow = FALSE, xanchor = 'right', yanchor = 'middle', font = list(color = '#D55E00', size = 12)),
            list(x = 0.98, y = xbar_chart$center, text = paste('CL:', round(xbar_chart$center, 2)), showarrow = FALSE, xanchor = 'right', yanchor = 'middle', font = list(color = '#009E73', size = 12))
         ))
)
