# Bikeshare.py
Importing US bike share data and computing descriptive statistics with a script that takes in raw input to create an interactive experience presenting these statistics.



import time #helps in time related fynctions
import pandas as pd #for data manipulation and analysis
import calendar #provides additional useful functions related to the calendar

''' 
the import function is used to import the modules or packages that will be
    used in the code 

'''
CITY_DATA = { 'chicago': 'chicago.csv',
              'new york city': 'new_york_city.csv',
              'washington': 'washington.csv' }

''' this is a dictionary containing all the city names as keys and the 
    corresponding .csv file which contains the data that will be used
'''
cities = ('chicago', 'new york city', 'washington')
months = ['january', 'february', 'march', 'april', 'may', 'june']
days = ['sunday', 'monday', 'tuesday', 'wednesday', 'thursday', 'friday', 'saturday']

''' 
these are list that will be used in the program
'''
def get_filters():

    '''
    this is the first function that's used to get the inputs of the user and 
    supports the other functions by providing these inputs
    
    Asks user to specify a city, month, and day to analyze.

    Returns:
        (str) city - name of the city to analyze
        (str) month - name of the month to filter by, or "all" to apply no month filter
        (str) day - name of the day of week to filter by, or "all" to apply no day filter
    '''

    print("\nHello! Let's explore some US bikeshare data!\n")
    # TO DO: get user input for city (chicago, new york city, washington). HINT: Use a while loop to handle invalid inputs

    while True:
        city = input("\nSelect which cities to explore : chicago, new york city or washington?\n").lower() 
        if city not in cities:
            print("\nplease, select a correct city name. \n")
        else:
            break
        
        ''' a while loop is used to iterate till a condtion is met because 
        the number of times needed to iterate through is unkown, the condition 
        here is that the input of the user must be a city in the list of cities.
        a lower function is added in case the use enter the city name with 
        a capatilized letter.
        the input function allows a user to insert a value into the program.
        '''
    # TO DO: get user input for month (all, january, february, ... , june)
    while True:
        month = input("\nWhich month to explore : from january till june. For all 6 months type : all\n").lower()
        if month != "all" and month not in months:
            print('\nplease enter a correct month name. \n')
        else:
            break
    # TO DO: get user input for day of week (all, monday, tuesday, ... sunday)
    while True:
        day = input("\nWhich day to explore. To explore all week please type : all\n").lower()
        if day != "all" and day not in days:
            print("\nplease enter a correct day.\n")
        else:
            break
    print('-'*40)
    return city, month, day


def load_data(city, month, day):
    """
    Loads data for the specified city and filters by month and day if applicable.

    Args:
        (str) city - name of the city to analyze
        (str) month - name of the month to filter by, or "all" to apply no month filter
        (str) day - name of the day of week to filter by, or "all" to apply no day filter
    Returns:
        df - Pandas DataFrame containing city data filtered by month and day
    """
    # load data file into a dataframe
    df = pd.read_csv(CITY_DATA[city])

    # convert the Start Time column to datetime
    df['Start Time'] = pd.to_datetime(df['Start Time'])

    # extract month and day of week from Start Time to create new columns
    df['month'] = df['Start Time'].dt.month
    df['day_of_week'] = df['Start Time'].dt.day_name()


    # filter by month if applicable
    if month != 'all':
        # use the index of the months list to get the corresponding int
        months = ['january', 'february', 'march', 'april', 'may', 'june']
        month = months.index(month) + 1
    
        # filter by month to create the new dataframe
        df = df[df['month'] == month]

    # filter by day of week if applicable
    if day != 'all':
        # filter by day of week to create the new dataframe
        df = df[df['day_of_week'] == day.title()]
    

    return df

def time_stats(df):
    """Displays statistics on the most frequent times of travel."""

    print('\nCalculating The Most Frequent Times of Travel...\n')
    start_time = time.time()

    # TO DO: display the most common month
    most_common_month = df["month"].mode()[0]
    print("\nThe most common month is:\n", calendar.month_name[most_common_month])
    '''
    calendar : This module allows you to output calendars
    '''
    # TO DO: display the most common day of week
    most_common_day = df["day_of_week"].mode()[0]
    print("\nThe most commmon day is:\n", most_common_day)

    # TO DO: display the most common start hour
    # convert the Start Time column to datetime
    df['Start Time'] = pd.to_datetime(df['Start Time'])
    # extract hour from the Start Time column to create an hour column
    df['hour'] = df['Start Time'].dt.hour
    most_common_start_hour = df['hour'].mode()[0]
    print("\nThe most common start hour is:\n", most_common_start_hour)

    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)

def trip_duration_stats(df):
    """Displays statistics on the total and average trip duration."""

    print('\nCalculating Trip Duration...\n')
    start_time = time.time()

    # TO DO: display total travel time
    total_travel_time = df["Trip Duration"].sum()
    print("\nThe total travel time is:\n", total_travel_time/3600, "hours or", total_travel_time/60, "minutes or", total_travel_time, "seconds.")

    # TO DO: display mean travel time
    mean_travel_time = df["Trip Duration"].mean()
    print("\nThe mean travel time is: \n", mean_travel_time/3600, "hours or", mean_travel_time/60, "minutes or", mean_travel_time, "seconds.")

    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)


def station_stats(df):
    """Displays statistics on the most popular stations and trip."""

    print('\nCalculating The Most Popular Stations and Trip...\n')
    start_time = time.time()

    # TO DO: display most commonly used start station
    most_commonly_used_start_station = df["Start Station"].mode()[0]
    print("\nThe most commonly used start station is:\n", most_commonly_used_start_station)

    # TO DO: display most commonly used end station
    most_commonly_used_end_station = df["End Station"].mode()[0]
    print("\nThe most commonly used end station is:\n", most_commonly_used_end_station)
    # TO DO: display most frequent combination of start station and end station trip
    most_frequent_combination = (df["Start Station"] + " - " + df["End Station"]).mode()[0]
    print("\nThe most frequent combination of start station and end station is:\n",most_frequent_combination)
    
    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)


def user_stats(df):
    """Displays statistics on bikeshare users."""

    print('\nCalculating User Stats...\n')
    start_time = time.time()

    # TO DO: Display counts of user types
    user_types = df['User Type'].value_counts()
    print("\nThe count of user types:\n", user_types)

    # TO DO: Display counts of gender
    if "Gender" in df:
        gender = df["Gender"].value_counts()
        print("\nThe counts of gender is:\n", gender)

    # TO DO: Display earliest, most recent, and most common year of birth
    if "Birth Year" in df:
        earliest_year = int(df["Birth Year"].min())
        most_recent_year = int(df["Birth Year"].max())
        most_common_year = int(df["Birth Year"].mode())
        print("\nThe earliest year is: \n", earliest_year, "\nThe most recent year is: \n", most_recent_year, "\nThe most common year is: ", most_common_year)

    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)

def raw_data(df):
    ''' displays the first 5 rows of data '''
    i = 0
    answer = input("please state if you would like to display the first 5 rows of data. yes/no: ").lower()
    if answer == "yes":
         print(df[i:i+5])
    elif answer == "no":
        print(" will not display 5 rows of data.")
    else:
        print("invalid choice...")
        answer = input("please state if you would like to display the first 5 rows of data. yes/no: ").lower()
def main():
    while True:
        city, month, day = get_filters()
        df = load_data(city, month, day)
        time_stats(df)
        trip_duration_stats(df)
        station_stats(df)
        user_stats(df)
        raw_data(df)

        restart = input('\nWould you like to restart? Enter yes or no.\n')
        if restart.lower() != 'yes':
            break


if __name__ == "__main__":
	main()
''' https://github.com/ozlerhakan/bikeshare/blob/master/bikeshare.py 
for plagiarism purposes i used this link to help understand the what i can do
''' 
