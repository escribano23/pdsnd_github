# pdsnd_github
GitHub project (Project 3) repository for PDSND
import time
import pandas as pd
import numpy as np

CITY_DATA = { 'Chicago': 'chicago.csv',
              'New york city': 'new_york_city.csv',
              'Washington': 'washington.csv' }

def get_filters():
    
    print('Hello! Let\'s explore some US bikeshare data!')  
    
    while True:
        city= ['Chicago','New york city','Washington']
        city= input("\n Which city are you choosing? (Chicago, New york city, Washington) \n")
        if city not in CITY_DATA.keys():
            print("\n Please enter a valid city name")
        else:
            break       
            
    while True:
        months= ['Jan','Feb','Mar','Apr','May','Jun','all']
        month= input("\n Which month are you choosing? Jan,Feb,Mar,Apr,May,Jun,all? \n")
        if month not in months:
            print("\n Please enter a valid month")
        else:
            break
        if month != 'all':
            months = ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun']
            month = months.index(month)

        # filter by month to create the new dataframe
        df = df[df['month'] == month]
            
    while True:
        days= ['Mo','Tu','We','Th','Fr','Sa','Su','all']
        day= input("\n Which day are you choosing? Mo,Tu,We,Th,Fr,Sa,Su,all? \n")
        if day not in days:
            print('\n Please enter a valid day')
        else:
            break
        if day != 'all':
            days = ['Mo','Tu','We','Th','Fr','Sa','Su']
            day = days.index(day)

        # filter by month to create the new dataframe
        df = df[df['day'] == day]
        
    print('-'*40)
    return city, month, day

def load_data(city, month, day):
    
    df = pd.read_csv(CITY_DATA[city])
    df['Start Time'] = pd.to_datetime(df['Start Time'])
    df['End Time'] = pd.to_datetime(df['End Time'])
    
    df['month'] = df['Start Time'].dt.month  
    df['day_of_week'] = df['Start Time'].dt.weekday_name
    df['hour'] = df['Start Time'].dt.hour
    
    return df

def time_stats(df):
    
    print('\nCalculating The Most Frequent Times of Travel...\n')
    start_time = time.time()
    
    print('The most common month is:{}'.format(df['month'].mode()[0]))
    
    print('The most common day of week is:{}'.format(df['day_of_week'].mode()[0]))
    
    print('The most common start hour is:{}'.format(df['hour'].mode()[0]))
    
    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)

def station_stats(df):
    
    print('\nCalculating The Most Popular Stations and Trip...\n')
    start_time = time.time()
    
    print('The most common Start Station is:{}'.format(df['Start Station'].mode()[0]))
    
    print('The most common End Station is:{}'.format(df['End Station'].mode()[0]))
    
    df['combination'] = df['Start Station'] + df['End Station']   
    print('The most common combination of Start and End Station is:{}'.format(df['combination'].mode()[0]))
    
    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)

def trip_duration_stats(df):
    
    print('\nCalculating Trip Duration...\n')
    start_time = time.time()
    
    print('Sum Total Travel Time is:',(df['Trip Duration'].sum()).round())
    print('Most Popular Total Travel Time:',(df['Trip Duration'].mode()).round())
    print('Mean Travel Time:',(df['Trip Duration'].mean()).round())
    
    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)

def user_stats(df,city):
    
    print('\nCalculating User Stats...\n')
    start_time = time.time()
    
    User_types = df['User Type'].value_counts()
    print("The count of user types is:" + str(User_types))
    
    if city != 'Washington':
        print(df['Gender'].value_counts().to_frame())
        earliest_birth = df['Birth Year'].min()
        most_recent_birth = df['Birth Year'].max()
        most_common_birth = df['Birth Year'].mode()[0]
        print('Earliest birth is: {}'.format(earliest_birth))
        print('Most recent birth is: {}'.format(most_recent_birth))
        print('Most common birth is: {}'.format(most_common_birth))
    else:
        print('no data for this')
        
    print("\nThis took %s seconds." % (time.time() - start_time))
    print('-'*40)

def display_raw_data(df):
    
    start_loc = 0
    while True:
        view_raw_data = input('\nWould you like to view 5 rows of individual trip data? Enter yes or no.\n')
        if view_raw_data.lower() != 'yes':
            return
        print(df.iloc[start_loc:start_loc+5])       
    view_data = input("Do you wish to continue?: ").lower()
    print(df)
    
def main():
    while True:
        city, month, day = get_filters()
        df = load_data(city, month, day)
        
        time_stats(df)
        station_stats(df)
        trip_duration_stats(df)
        user_stats(df,city)
        display_raw_data(df)
                
        restart = input('\nWould you like to restart? Enter yes or no.\n')
        if restart.lower() != 'yes':
            break            
            
if __name__ == "__main__":
	main()
