from urllib import request
import json

class MutualFundProfitCalculator:
    def __init__(self, scheme_code: int, start_date: str, end_date: str, capital: float = 1000000.0):
        self.scheme_code = scheme_code
        self.start_date = start_date
        self.end_date = end_date
        self.capital = capital

    def fetch_nav_data(self):
        api_url = f"https://api.mfapi.in/mf/{self.scheme_code}"
        response = request.urlopen(api_url)
        # response = requests.get(api_url)
        if response.getcode() == 200:
            data = json.loads(response.read().decode('utf-8'))
            return data
        else:
            raise Exception(f"Failed to fetch NAV data. Status code: {response.status_code}")

    def calculate_profit(self):
        nav_data = self.fetch_nav_data()
        for d in nav_data["data"]:
          if d["date"] == self.start_date:
            first_data = d["nav"]
          if d["date"] == self.end_date:
            second_data = d["nav"]

        units_allotted = self.capital / float(first_data)
        value_end_date = units_allotted * float(second_data)
        net_profit = value_end_date - self.capital
        return net_profit


