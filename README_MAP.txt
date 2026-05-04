How to open the map (Windows, Chrome)

1) Files created:
   - borno_schools.geojson   (OSM school points)
   - borno_map.html          (Leaflet map that reads the geojson)

2) Start a local web server in the folder (important: browsers block fetch() for local files when opened with file://):
   - Open PowerShell or Command Prompt, change to the project folder e.g.:
       cd "C:\Users\YourName\path\to\CrashCourse1Exam"
   - Run a simple server (Python required):
       python -m http.server 8000
     or (if python3 command is python3):
       python3 -m http.server 8000

3) Open Chrome and go to:
       http://localhost:8000/borno_map.html

4) To add conflict data (ACLED, UCDP):
   - Download CSV from ACLED/UCDP (they provide CSV or Excel). Convert to GeoJSON (see instructions below).
   - Save as borno_conflict.geojson in the same folder. Reload the page to see conflict events.

Quick CSV -> GeoJSON using QGIS or simple python (example):
   python -c "import pandas as pd, json
from pathlib import Path
p=Path('borno_conflict.csv')
d=pd.read_csv(p)
features=[]
for _,r in d.iterrows():
  try:
    lon=float(r['longitude']); lat=float(r['latitude'])
  except:
    continue
  props=r.to_dict()
  features.append({'type':'Feature','geometry':{'type':'Point','coordinates':[lon,lat]},'properties':props})
geo={'type':'FeatureCollection','features':features}
open('borno_conflict.geojson','w').write(json.dumps(geo))"

If you want, I can also try to download UCDP conflict events for Nigeria and convert them into borno_conflict.geojson for you.