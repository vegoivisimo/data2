# 0. Importar las bibliotecas necesarias


```
import pandas as pd
import yfinance as yf
from bs4 import BeautifulSoup
import requests
import matplotlib.pyplot as plt
```

# 1. Extraer los datos de la acción de Tesla (TSLA) usando yfinance


```
tesla = yf.Ticker("TSLA")
tesla_data = tesla.history(period="max")
tesla_data.reset_index(inplace=True)
tesla_data.head()
```

# 2. Extraer los datos de ingresos de Tesla a través de Web Scraping


```
url_tesla_rev = "https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0220EN-SkillsNetwork/labs/project/revenue.htm"
tables = pd.read_html(url_tesla_rev)
tesla_rev = tables[0].copy()
tesla_rev.columns = ["Date", "Revenue"]
tesla_rev = tesla_rev[tesla_rev["Revenue"] != "Revenue"]
tesla_rev["Date"] = pd.to_datetime(tesla_rev["Date"])
tesla_rev["Revenue"] = tesla_rev["Revenue"].str.replace(r"[\$,]", "", regex=True).astype(float)
tesla_rev.head()
```

# 3. Extraer los datos de la acción de GameStop (GME) usando yfinance


```
gme = yf.Ticker("GME")
gme_data = gme.history(period="max")
gme_data.reset_index(inplace=True)
gme_data.head()
```

# 4. Extraer los datos de ingresos de GameStop a través de Web Scraping


```
url_gme_rev = "https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0220EN-SkillsNetwork/labs/project/stock.html"
tables = pd.read_html(url_gme_rev)
gme_rev = tables[0].copy()
gme_rev.columns = ["Date", "Revenue"]
gme_rev = gme_rev[gme_rev["Revenue"] != "Revenue"]
gme_rev["Date"] = pd.to_datetime(gme_rev["Date"])
gme_rev["Revenue"] = gme_rev["Revenue"].str.replace(r"[\$,]", "", regex=True).astype(float)
gme_rev.head()
```

# 5. Dashboard de Tesla: Precio de la acción vs Ingresos usando pandas.plot()


```
tesla_data.reset_index(inplace=True)
ax1 = tesla_data.plot(
    x="Date", 
    y="Close",
    title="Tesla: Precio de Cierre vs Ingresos Anuales",
    figsize=(10, 5),
    legend=False
)

ax1.set_ylabel("Precio de Cierre (USD)")
tesla_rev.plot(
    x="Date", 
    y="Revenue", 
    kind="bar",
    ax=ax1.twinx(), 
    alpha=0.3,
    legend=False
)

ax1.right_ax = ax1.get_shared_x_axes()
ax1.figure.tight_layout()
```
![png](output_4_0.png)
# 6. Dashboard de GameStop: Precio de la acción vs Ingresos usando pandas.plot()


```
ax2 = gme_data.plot(
    x="Date", 
    y="Close",
    title="GameStop: Precio de Cierre vs Ingresos Anuales",
    figsize=(10, 5),
    legend=False
)

ax2.set_ylabel("Precio de Cierre (USD)")
gme_rev.plot(
    x="Date", 
    y="Revenue", 
    kind="bar",
    ax=ax2.twinx(), 
    alpha=0.3,
    legend=False
)

ax2.figure.tight_layout()
```
![png](output_5_0.png)
