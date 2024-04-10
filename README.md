from datetime import datetime
import requests

class MutualFundProfitCalculator:
    def __init__(self, scheme_code: int, start_date: str, end_date: str, capital: float = 1000000.0):
        self.scheme_code = scheme_code
        self.start_date = datetime.strptime(start_date, "%d-%m-%Y")
        self.end_date = datetime.strptime(end_date, "%d-%m-%Y")
        self.capital = capital
    
    def fetch_nav_data(self):
        start_date_str = self.start_date.strftime('%d-%m-%Y')
        end_date_str = self.end_date.strftime('%d-%m-%Y')
        api_url = f"https://api.mfapi.in/mf/{self.scheme_code}?start={start_date_str}&end={end_date_str}"
        response = requests.get(api_url)
        if response.status_code == 200:
            data = response.json()
            return data
        else:
            raise Exception(f"Failed to fetch NAV data. Status code: {response.status_code}")

    def calculate_profit(self):
        nav_data = self.fetch_nav_data()
        nav_start_date = nav_data['data'][0]['nav']
        nav_end_date = nav_data['data'][-1]['nav']
        units_allotted = self.capital / nav_start_date
        value_end_date = units_allotted * nav_end_date
        net_profit = value_end_date - self.capital
        return net_profit
