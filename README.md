# sea_level_predictor.py
import pandas as pd
import matplotlib.pyplot as plt
from scipy.stats import linregress

def predict_sea_level():
    # Load the data
    data = pd.read_csv('epa-sea-level.csv')

    # Scatter plot of the data
    plt.scatter(data['Year'], data['CSIRO Adjusted Sea Level'])

    # Perform linear regression for all data from 1880 to the most recent year
    slope, intercept, r_value, p_value, std_err = linregress(data['Year'], data['CSIRO Adjusted Sea Level'])
    
    # Create a line of best fit for all years (1880 to the latest)
    years_extended = range(1880, 2051)  # Extend to 2050
    sea_levels_extended = [slope * year + intercept for year in years_extended]
    plt.plot(years_extended, sea_levels_extended, label="Fit: All data", color="blue")

    # Perform linear regression for data from 2000 onwards
    data_post_2000 = data[data['Year'] >= 2000]
    slope_2000, intercept_2000, _, _, _ = linregress(data_post_2000['Year'], data_post_2000['CSIRO Adjusted Sea Level'])

    # Create a line of best fit for the years 2000 to 2050
    sea_levels_post_2000 = [slope_2000 * year + intercept_2000 for year in years_extended]
    plt.plot(years_extended, sea_levels_post_2000, label="Fit: 2000 to Present", color="red")

    # Adding labels and title
    plt.xlabel('Year')
    plt.ylabel('Sea Level (inches)')
    plt.title('Rise in Sea Level')

    # Adding a legend
    plt.legend()

    # Save the plot as an image
    plt.savefig('sea_level_rise.png')

    # Show the plot
    plt.show()

# Call the function to generate the plot
predict_sea_level()
