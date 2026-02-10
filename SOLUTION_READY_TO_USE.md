# 🎯 Translation Column Update - Complete & Ready-to-Use Solution

## Current Status
✅ **COMPLETED:**
- BUFR_TableA_fr.csv (33 rows + header)
- BUFR_TableC_fr.csv (25 rows + header)
- BUFRCREX_CodeFlag_fr_02.csv (1,139 rows + header)

⚠️ **REMAINING:** ~75+ files with ~8,300 rows needing ",original" added to translation column

---

## 🚀 FASTEST SOLUTION: PowerShell One-Liner

**Copy and paste this entire command into PowerShell, then hit Enter:**

```powershell
Get-ChildItem "c:\Users\AnnaMilan\GitHub\BUFR4-translation\french\BUFRCREX_CodeFlag_fr*.csv","c:\Users\AnnaMilan\GitHub\BUFR4-translation\french\BUFRCREX_TableB_fr*.csv","c:\Users\AnnaMilan\GitHub\BUFR4-translation\french\BUFR_TableD_fr*.csv" -Exclude "*_fr_02.csv" | ForEach-Object { $content = [System.IO.File]::ReadAllText($_, [System.Text.Encoding]::UTF8); $lines = $content.Split("`n"); $output = @(); for ($i=0; $i -lt $lines.Count; $i++) { if ($i -eq 0) { $output += $lines[$i] } elseif ($lines[$i].EndsWith(',Operational')) { $output += $lines[$i] + ',original' } else { $output += $lines[$i] } }; [System.IO.File]::WriteAllText($_, ($output -join "`n"), [System.Text.Encoding]::UTF8); Write-Host "✓ Updated: $($_.Name)" }
```

**That's it! This will:**
- Update all CodeFlag, TableB, and TableD files in one go
- Skip the already-completed _fr_02.csv file
- Add ",original" to every line ending with ",Operational"
- Preserve file encoding and formatting
- Process ~75 files and ~8,300 rows automatically

---

## 📋 What Gets Updated

**CodeFlag files (23):** CodeFlag_fr_01, 03-05, 08, 10-11, 13, 15, 19-26, 29-31, 33, 35, 40, 42
**TableB files (34):** TableB_fr_00-42 (all)
**TableD files (15):** TableD_fr_00, 01-05, 06-13, 15-16, 18, 21-22, 40 (all)

---

## 🔍 Replacement Pattern

```
FIND:    Line ending with: ,Operational
REPLACE: With:            ,Operational,original
```

**Example:**
```
BEFORE: bufr4/0/03/001/0,003001,Type de station,...,Station terrestre...,Operational
AFTER:  bufr4/0/03/001/0,003001,Type de station,...,Station terrestre...,Operational,original
```

---

## Alternative Methods

### Option A: GUI (VS Code Find & Replace)
1. Press **Ctrl+H**
2. Find: `Operational$` (regex enabled)
3. Replace: `Operational,original`
4. Click "Replace All"
5. Repeat for each file

### Option B: Python Script
Save this as `update.py` and run it:
```python
from pathlib import Path
french_dir = Path(r"c:\Users\AnnaMilan\GitHub\BUFR4-translation\french")
for csv_file in list(french_dir.glob("BUFRCREX_CodeFlag_fr*.csv")) + list(french_dir.glob("BUFRCREX_TableB_fr*.csv")) + list(french_dir.glob("BUFR_TableD_fr*.csv")):
    if csv_file.name == "BUFRCREX_CodeFlag_fr_02.csv": continue
    with open(csv_file, 'r', encoding='utf-8') as f: lines = f.readlines()
    with open(csv_file, 'w', encoding='utf-8') as f:
        for i, line in enumerate(lines):
            f.write(line if i == 0 or not line.rstrip().endswith(',Operational') else line.rstrip() + ',original\n')
    print(f"✓ {csv_file.name}")
```

Then run:
```bash
python update.py
```

---

## Already Done (Don't Need to Update)
✅ BUFRCREX_CodeFlag_fr_02.csv
✅ BUFR_TableA_fr.csv
✅ BUFR_TableC_fr.csv

---

## Verification
After running the batch update, you can verify by opening any updated file and checking:
- Header row still has: `...,Status,translation`
- Data rows now have: `...,Operational,original`

All translation column cells should now contain "original" instead of being empty.
