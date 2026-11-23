# PrizeVault — Intelligent Contract (ஒவ்வொரு step-ஆ)

## STEP 1 — Folder
`PrizeVault.zip`-ஐ right-click → **Extract All** → Location-ல `D:\` கொடுங்க. கடைசியா `D:\PrizeVault` ரெடியா இருக்கணும்.

## STEP 2 — GitHub push (Studio test-க்கு முன்னாடியே இதை செய்யணும்)
```
cd D:\PrizeVault; git init; git add .; git commit -m "PrizeVault v1"; git branch -M main; gh repo create prizevault --public --source . --push
```
`gh` வேலை செய்யலைன்னா: github.com-ல **prizevault** public repo create பண்ணுங்க. அப்புறம்:
```
cd D:\PrizeVault; git remote add origin https://github.com/gurusankar55/prizevault.git; git push -u origin main
```
**Check:** https://raw.githubusercontent.com/gurusankar55/prizevault/main/demo/results-published.html open ஆகணும்.

## STEP 3 — Studio-ல 2 deploy
`contracts\prize_vault.py` code முழுசையும் paste பண்ணுங்க. Header-ஐ மாத்த வேண்டாம்.
- `demo_mode` = **true** வச்சு Deploy → address-ஐ "DEMO"ன்னு எழுதி வச்சுக்கோங்க.
- `demo_mode` = **false** வச்சு Deploy → address-ஐ "LIVE"ன்னு எழுதி வச்சுக்கோங்க.

## STEP 4 — Studio test (DEMO contract-ல)
URL ஆரம்பம்: `https://raw.githubusercontent.com/gurusankar55/prizevault/main/demo/`

**Test A — Results வந்தது, winner-க்கு பணம்**
- `create_vault`: name = `Chain Hack 2026`, results_url = ஆரம்பம் + `results-published.html`, split_bp = `5000,3000,2000`, register_for = `2`, results_within = `2`, claim_window = `3`, Value = `10`
- **உடனே** `register`: vault_id = `1`, project_name = `TrustLink`, project_url = ஆரம்பம் + `project-trustlink.html`
- 2 நிமிஷம் கழிச்சு `check_results(1)` → `RESULTS_PUBLISHED_CLAIMS_OPEN`
- `claim_prize(1, 2)` → `CLAIM_REJECTED_NO_MATCH` (TrustLink 2nd place இல்லை, அதனால reject. இது சரிதான்)
- `claim_prize(1, 1)` → `PRIZE_PAID_PLACE_1`
- 3 நிமிஷம் கழிச்சு `close(1)` → மீதி 5 GEN organizer-க்கு திரும்ப போகும்

**Test B — Results வரலை → உண்மையான team-க்கு pool**
- `create_vault`: results_url = `results-pending.html`, மீதி எல்லாம் Test A மாதிரியே (Value 10) → vault 2
- உடனே `register(2, "TrustLink", ஆரம்பம் + project-trustlink.html)`
- 2 நிமிஷம் கழிச்சு `check_results(2)` பண்ணுங்க. அப்புறம் **ஒவ்வொரு 1 நிமிஷத்துக்கும்** திரும்ப பண்ணுங்க, `NO_RESULTS_VERIFY_ENTRIES` வர்ற வரைக்கும் (4–5 தடவை ஆகும்)
- `verify_entry(2)` → `ENTRY_VERIFIED`
- 3 நிமிஷம் கழிச்சு `claim_share(2)` → 10 GEN

**Test C — Fake page reject**
- vault 3: `results-pending.html` + `register(3, "Ghost", ஆரம்பம் + project-empty.html)`
- Test B மாதிரியே `NO_RESULTS` வர்ற வரைக்கும் check பண்ணுங்க → `verify_entry(3)` → `VERIFY_REJECTED_NOT_REAL`
- 3 நிமிஷம் கழிச்சு `close(3)` → 10 GEN organizer-க்கு திரும்ப போகும் (verified team யாரும் இல்லை)

**Test D — தப்பான split**
- `create_vault` split_bp = `5000,3000` வச்சு பண்ணுங்க → error வரணும் (total 10000 இல்லை). Error வந்தா test pass.

நேரம் மிச்சம் பண்ண: A, B, C மூணு vault-ஐயும் முதல்ல create பண்ணி register பண்ணிடுங்க. அப்புறம் ஒவ்வொண்ணா check பண்ணுங்க.

Screenshots-ஐ `D:\PrizeVault\evidence\`-ல போடுங்க: `A_paid.png`, `A_close.png`, `B_share.png`, `C_rejected.png`, `D_split_error.png`.

ஒரு call "undetermined"ன்னு வந்தா, 1 நிமிஷம் கழிச்சு அதையே திரும்ப பண்ணுங்க.

## STEP 5 — Addresses + screenshots push
`DEMO_ADDR`, `LIVE_ADDR` இடத்துல உங்க address-களை போட்டு ஓட்டுங்க:
```
cd D:\PrizeVault; (Get-Content SUBMIT_TEXT.md) -replace 'Demo contract \(Studio\): 0x_+','Demo contract (Studio): DEMO_ADDR' -replace 'Live contract \(Studio\): 0x_+','Live contract (Studio): LIVE_ADDR' | Set-Content SUBMIT_TEXT.md; git add .; git commit -m "Studio test evidence and addresses"; git push
```

## STEP 6 — Portal submit
**Intelligent Contracts** category-ல submit பண்ணுங்க. `SUBMIT_TEXT.md`-ல இருக்கிற Title, Description, GitHub link, contract address-ஐ copy பண்ணுங்க.
