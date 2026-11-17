# Test contracts for Clausify

Twenty synthetic agreements for checking the deployed app. Every clause is planted, so
the expected findings below are ground truth, not guesses. Upload the `.txt` file or paste
its contents, then compare what the app reports against the table for that contract.

These are fabricated documents written for testing. They are not real agreements, not
drawn from CUAD, and carry no legal meaning.

## At a glance

| # | File | Chars | Clauses | High | What it tests |
|---|---|---:|---:|---:|---|
| 1 | `01_software_distribution.txt` | 9,683 | 19 | 8 | Dense high-risk distribution deal. Should surface 7-8 High. |
| 2 | `02_mutual_nda.txt` | 4,165 | 6 | 0 | Benign NDA. Should be mostly Low/Medium, NO High. Good false-positive check. |
| 3 | `03_employment.txt` | 5,133 | 8 | 2 | Employment. Non-Compete + IP Assignment are the two High items. |
| 4 | `04_saas_subscription.txt` | 6,231 | 11 | 0 | Standard SaaS. Medium-heavy, no High. Tests the Medium band. |
| 5 | `05_reseller.txt` | 6,006 | 10 | 3 | Reseller with commercial restrictions. 3 High + several rare Medium. |
| 6 | `06_consulting.txt` | 5,633 | 8 | 2 | Consulting. IP Assignment + Uncapped Liability are the High pair. |
| 7 | `07_patent_license.txt` | 5,417 | 9 | 1 | IP licensing. Tests Joint IP and Affiliate License-Licensor (rare categories). |
| 8 | `08_supply.txt` | 5,765 | 9 | 2 | Supply chain. 2 High (Minimum Commitment, MFN). |
| 9 | `09_joint_venture.txt` | 5,673 | 8 | 1 | JV. Joint IP is the headline; Exclusivity is the only High. |
| 10 | `10_franchise.txt` | 6,740 | 11 | 3 | Franchise. 3 High and heavy control clauses. |
| 11 | `11_short_letter.txt` | 2,168 | 4 | 0 | SHORT (1 window). Tests fast path and short-input handling. |
| 12 | `12_trademark_license.txt` | 5,390 | 9 | 0 | Trademark. Tests both Affiliate License categories (rare tail). |
| 13 | `13_manufacturing.txt` | 6,116 | 9 | 2 | Manufacturing. Minimum Commitment + IP Assignment are High. |
| 14 | `14_marketing.txt` | 5,247 | 8 | 1 | Marketing. Exclusivity is the single High. |
| 15 | `15_escrow_support.txt` | 5,344 | 8 | 0 | RARE TAIL TEST. Source Code Escrow has only 13 positives in all of CUAD, expect a miss. |
| 16 | `16_agency.txt` | 5,340 | 7 | 1 | Agency. Tests Covenant Not To Sue (uncommon). |
| 17 | `17_benign_control.txt` | 5,149 | 7 | 0 | NEGATIVE CONTROL. Contains NO High-risk clause. Any High card here is a false positive. |
| 18 | `18_long_contract.txt` | 48,024 | 11 | 4 | LONG (>45,500 chars). Trips the 30-window cap: late clauses should be MISSED. |
| 19 | `19_maximal_risk.txt` | 6,245 | 10 | 8 | STRESS TEST. ALL 8 High categories present. Measures High-risk recall directly. |
| 20 | `20_equipment_lease.txt` | 5,997 | 10 | 0 | Lease. All Medium/Low, no High. Second false-positive check. |

## How to read the results

The app runs the transformer alone at threshold 0.50, which over-fires: measured precision
is 0.562 against recall 0.908. **Expect false positives.** A category the table does not list
turning up as a card is normal and not a bug. A High-risk clause that is listed and does *not*
appear is the failure that matters, because that is the warning a signer would never see.

Three contracts are deliberate controls:

- **02, 17 and 20** contain no High-risk clause at all. Any High card is a false positive.
- **19** contains all eight High categories. The fraction it finds is High-risk recall, which
  the report puts at 0.841 for the transformer alone.
- **18** is 48,024 characters, past the app's 30-window cap at roughly 45,500. The Liquidated
  Damages and Most Favored Nation clauses sit at characters 47,145 and 47,594, beyond the cap,
  so they **should be missed**. If the app reports them, the cap is not working as documented.

Also worth watching: **15** plants Source Code Escrow, which has only 13 positive contracts in
all of CUAD. A miss there is expected and is the rare-tail limitation, not a defect.


## Expected findings per contract


### 1. `01_software_distribution.txt`

*MASTER SOFTWARE LICENSE AND DISTRIBUTION AGREEMENT* , 9,683 characters, about 7 windows.

Dense high-risk distribution deal. Should surface 7-8 High.

| Planted clause | Expected risk |
|---|---|
| Exclusivity | **High** |
| Ip Ownership Assignment | **High** |
| Irrevocable Or Perpetual License | **High** |
| Liquidated Damages | **High** |
| Minimum Commitment | **High** |
| Most Favored Nation | **High** |
| Non-Compete | **High** |
| Uncapped Liability | **High** |
| Anti-Assignment | Medium |
| Audit Rights | Medium |
| Cap On Liability | Medium |
| Change Of Control | Medium |
| Notice Period To Terminate Renewal | Medium |
| Renewal Term | Medium |
| Termination For Convenience | Medium |
| Expiration Date | Low |
| Governing Law | Low |
| Insurance | Low |
| License Grant | Low |

### 2. `02_mutual_nda.txt`

*MUTUAL NON-DISCLOSURE AGREEMENT* , 4,165 characters, about 3 windows.

Benign NDA. Should be mostly Low/Medium, NO High. Good false-positive check.

| Planted clause | Expected risk |
|---|---|
| Anti-Assignment | Medium |
| No-Solicit Of Employees | Medium |
| Non-Disparagement | Medium |
| Effective Date | Low |
| Expiration Date | Low |
| Governing Law | Low |

### 3. `03_employment.txt`

*EXECUTIVE EMPLOYMENT AGREEMENT* , 5,133 characters, about 4 windows.

Employment. Non-Compete + IP Assignment are the two High items.

| Planted clause | Expected risk |
|---|---|
| Ip Ownership Assignment | **High** |
| Non-Compete | **High** |
| Competitive Restriction Exception | Medium |
| No-Solicit Of Customers | Medium |
| No-Solicit Of Employees | Medium |
| Non-Disparagement | Medium |
| Effective Date | Low |
| Governing Law | Low |

### 4. `04_saas_subscription.txt`

*SOFTWARE-AS-A-SERVICE SUBSCRIPTION AGREEMENT* , 6,231 characters, about 4 windows.

Standard SaaS. Medium-heavy, no High. Tests the Medium band.

| Planted clause | Expected risk |
|---|---|
| Anti-Assignment | Medium |
| Audit Rights | Medium |
| Cap On Liability | Medium |
| Notice Period To Terminate Renewal | Medium |
| Renewal Term | Medium |
| Termination For Convenience | Medium |
| Expiration Date | Low |
| Governing Law | Low |
| Insurance | Low |
| License Grant | Low |
| Warranty Duration | Low |

### 5. `05_reseller.txt`

*AUTHORIZED RESELLER AGREEMENT* , 6,006 characters, about 4 windows.

Reseller with commercial restrictions. 3 High + several rare Medium.

| Planted clause | Expected risk |
|---|---|
| Exclusivity | **High** |
| Minimum Commitment | **High** |
| Most Favored Nation | **High** |
| Audit Rights | Medium |
| Price Restrictions | Medium |
| Renewal Term | Medium |
| Revenue/Profit Sharing | Medium |
| Rofr/Rofo/Rofn | Medium |
| Volume Restriction | Medium |
| Governing Law | Low |

### 6. `06_consulting.txt`

*PROFESSIONAL SERVICES AND CONSULTING AGREEMENT* , 5,633 characters, about 4 windows.

Consulting. IP Assignment + Uncapped Liability are the High pair.

| Planted clause | Expected risk |
|---|---|
| Ip Ownership Assignment | **High** |
| Uncapped Liability | **High** |
| Post-Termination Services | Medium |
| Termination For Convenience | Medium |
| Effective Date | Low |
| Governing Law | Low |
| Insurance | Low |
| Warranty Duration | Low |

### 7. `07_patent_license.txt`

*PATENT LICENSE AND TECHNOLOGY TRANSFER AGREEMENT* , 5,417 characters, about 4 windows.

IP licensing. Tests Joint IP and Affiliate License-Licensor (rare categories).

| Planted clause | Expected risk |
|---|---|
| Irrevocable Or Perpetual License | **High** |
| Affiliate License-Licensor | Medium |
| Audit Rights | Medium |
| Covenant Not To Sue | Medium |
| Joint Ip Ownership | Medium |
| Non-Transferable License | Medium |
| Revenue/Profit Sharing | Medium |
| Governing Law | Low |
| License Grant | Low |

### 8. `08_supply.txt`

*MASTER SUPPLY AGREEMENT* , 5,765 characters, about 4 windows.

Supply chain. 2 High (Minimum Commitment, MFN).

| Planted clause | Expected risk |
|---|---|
| Minimum Commitment | **High** |
| Most Favored Nation | **High** |
| Change Of Control | Medium |
| Notice Period To Terminate Renewal | Medium |
| Renewal Term | Medium |
| Volume Restriction | Medium |
| Governing Law | Low |
| Insurance | Low |
| Warranty Duration | Low |

### 9. `09_joint_venture.txt`

*JOINT DEVELOPMENT AND COLLABORATION AGREEMENT* , 5,673 characters, about 4 windows.

JV. Joint IP is the headline; Exclusivity is the only High.

| Planted clause | Expected risk |
|---|---|
| Exclusivity | **High** |
| Joint Ip Ownership | Medium |
| No-Solicit Of Employees | Medium |
| Post-Termination Services | Medium |
| Revenue/Profit Sharing | Medium |
| Rofr/Rofo/Rofn | Medium |
| Effective Date | Low |
| Governing Law | Low |

### 10. `10_franchise.txt`

*FRANCHISE AGREEMENT* , 6,740 characters, about 5 windows.

Franchise. 3 High and heavy control clauses.

| Planted clause | Expected risk |
|---|---|
| Exclusivity | **High** |
| Minimum Commitment | **High** |
| Non-Compete | **High** |
| Anti-Assignment | Medium |
| Audit Rights | Medium |
| Change Of Control | Medium |
| Price Restrictions | Medium |
| Renewal Term | Medium |
| Revenue/Profit Sharing | Medium |
| Governing Law | Low |
| Insurance | Low |

### 11. `11_short_letter.txt`

*LETTER AGREEMENT REGARDING EVALUATION ACCESS* , 2,168 characters, about 2 windows.

SHORT (1 window). Tests fast path and short-input handling.

| Planted clause | Expected risk |
|---|---|
| Non-Transferable License | Medium |
| Expiration Date | Low |
| Governing Law | Low |
| License Grant | Low |

### 12. `12_trademark_license.txt`

*TRADEMARK LICENSE AGREEMENT* , 5,390 characters, about 4 windows.

Trademark. Tests both Affiliate License categories (rare tail).

| Planted clause | Expected risk |
|---|---|
| Affiliate License-Licensor | Medium |
| Audit Rights | Medium |
| Non-Disparagement | Medium |
| Non-Transferable License | Medium |
| Price Restrictions | Medium |
| Revenue/Profit Sharing | Medium |
| Affiliate License-Licensee | Low |
| Governing Law | Low |
| License Grant | Low |

### 13. `13_manufacturing.txt`

*CONTRACT MANUFACTURING AGREEMENT* , 6,116 characters, about 4 windows.

Manufacturing. Minimum Commitment + IP Assignment are High.

| Planted clause | Expected risk |
|---|---|
| Ip Ownership Assignment | **High** |
| Minimum Commitment | **High** |
| Audit Rights | Medium |
| Change Of Control | Medium |
| Volume Restriction | Medium |
| Expiration Date | Low |
| Governing Law | Low |
| Insurance | Low |
| Warranty Duration | Low |

### 14. `14_marketing.txt`

*CO-MARKETING AND BRAND PARTNERSHIP AGREEMENT* , 5,247 characters, about 4 windows.

Marketing. Exclusivity is the single High.

| Planted clause | Expected risk |
|---|---|
| Exclusivity | **High** |
| No-Solicit Of Customers | Medium |
| Non-Disparagement | Medium |
| Renewal Term | Medium |
| Revenue/Profit Sharing | Medium |
| Termination For Convenience | Medium |
| Effective Date | Low |
| Governing Law | Low |

### 15. `15_escrow_support.txt`

*SOFTWARE SUPPORT AND SOURCE CODE ESCROW AGREEMENT* , 5,344 characters, about 4 windows.

RARE TAIL TEST. Source Code Escrow has only 13 positives in all of CUAD, expect a miss.

| Planted clause | Expected risk |
|---|---|
| Cap On Liability | Medium |
| Notice Period To Terminate Renewal | Medium |
| Post-Termination Services | Medium |
| Renewal Term | Medium |
| Source Code Escrow | Medium |
| Governing Law | Low |
| Third Party Beneficiary | Low |
| Warranty Duration | Low |

### 16. `16_agency.txt`

*SALES AGENCY AND REPRESENTATION AGREEMENT* , 5,340 characters, about 4 windows.

Agency. Tests Covenant Not To Sue (uncommon).

| Planted clause | Expected risk |
|---|---|
| Exclusivity | **High** |
| Anti-Assignment | Medium |
| Covenant Not To Sue | Medium |
| No-Solicit Of Customers | Medium |
| Revenue/Profit Sharing | Medium |
| Termination For Convenience | Medium |
| Governing Law | Low |

### 17. `17_benign_control.txt`

*FACILITIES ACCESS AND SERVICES AGREEMENT* , 5,149 characters, about 4 windows.

NEGATIVE CONTROL. Contains NO High-risk clause. Any High card here is a false positive.

| Planted clause | Expected risk |
|---|---|
| Effective Date | Low |
| Expiration Date | Low |
| Governing Law | Low |
| Insurance | Low |
| License Grant | Low |
| Third Party Beneficiary | Low |
| Warranty Duration | Low |

### 18. `18_long_contract.txt`

*MASTER SERVICES, LICENSE AND DISTRIBUTION AGREEMENT (LONG FORM)* , 48,024 characters, about 32 windows.

LONG (>45,500 chars). Trips the 30-window cap: late clauses should be MISSED.

| Planted clause | Expected risk |
|---|---|
| Exclusivity | **High** |
| Ip Ownership Assignment | **High** |
| Minimum Commitment | **High** |
| Non-Compete | **High** |
| Audit Rights | Medium |
| Cap On Liability | Medium |
| Renewal Term | Medium |
| Expiration Date | Low |
| Governing Law | Low |
| Insurance | Low |
| License Grant | Low |

### 19. `19_maximal_risk.txt`

*STRATEGIC ALLIANCE AND EXCLUSIVE SUPPLY AGREEMENT* , 6,245 characters, about 4 windows.

STRESS TEST. ALL 8 High categories present. Measures High-risk recall directly.

| Planted clause | Expected risk |
|---|---|
| Exclusivity | **High** |
| Ip Ownership Assignment | **High** |
| Irrevocable Or Perpetual License | **High** |
| Liquidated Damages | **High** |
| Minimum Commitment | **High** |
| Most Favored Nation | **High** |
| Non-Compete | **High** |
| Uncapped Liability | **High** |
| Effective Date | Low |
| Governing Law | Low |

### 20. `20_equipment_lease.txt`

*EQUIPMENT LEASE AND MAINTENANCE AGREEMENT* , 5,997 characters, about 4 windows.

Lease. All Medium/Low, no High. Second false-positive check.

| Planted clause | Expected risk |
|---|---|
| Anti-Assignment | Medium |
| Change Of Control | Medium |
| Notice Period To Terminate Renewal | Medium |
| Post-Termination Services | Medium |
| Renewal Term | Medium |
| Termination For Convenience | Medium |
| Expiration Date | Low |
| Governing Law | Low |
| Insurance | Low |
| Warranty Duration | Low |
