# Manual semantic-search evaluation

Collection: `document_chunks` in Qdrant. **820 points; 820 indexed vectors; green status.** HNSW uses 384-dimensional cosine vectors (`m=16`, `ef_construct=100`). Default score threshold: 0.35.

**Judging rule:** Y means at least one of the top three results is from the expected source and contains evidence responsive to the query. These are manual relevance judgments, not automatically inferred labels. Query 8 also applies `doc_type=docx`.

| Query | Best result relevant (Y/N; within top 3) | Top-three sources, locator, score |
|---|---:|---|
| How does the World Bank proactively disclose information and make its document database publicly accessible? | Y | `wb_ati_fy2022.pdf` p.2 (0.771); `wb_ati_fy2011.pdf` p.32 (0.769); `wb_ati_fy2011.pdf` p.26 (0.765) |
| How many information requests did the World Bank receive in FY 2015 and how quickly did it respond? | Y | `wb_ati_fy2014.pdf` p.18 (0.789); `wb_ati_fy2015.pdf` p.24 (0.789); `wb_access_to_information_fy2015.docx` section 1 (0.784) |
| When can the World Bank disclose restricted information if the public benefit outweighs the potential harm? | Y | `wb_ati_fy2015.pdf` p.27 (0.740); `wb_ati_fy2014.pdf` p.22 (0.697); `wb_ati_fy2013.pdf` p.12 (0.696) |
| What appeals process is available for information requests denied under the Access to Information policy? | Y | `wb_ati_fy2012.pdf` p.52 (0.774); `wb_ati_fy2012.pdf` p.51 (0.734); `wb_ati_fy2013.pdf` p.43 (0.717) |
| How does the World Bank use outreach, consultations and citizen engagement to improve transparency? | Y | `wb_ati_fy2020.pdf` p.2 (0.725); `wb_ati_fy2013.pdf` p.18 (0.722); `wb_ati_fy2013.pdf` p.20 (0.721) |
| What strategy does WHO use to set publication priorities and monitor compliance with publishing policy? | Y | `who_publications_policy.pdf` p.56 (0.661); `who_publications_policy.pdf` p.49 (0.659); `who_publications_policy.docx` section 1 (0.657) |
| What role does the WHO Guidelines Review Committee play in developing guidelines? | Y | `who_publications_policy.pdf` p.50 (0.613); `who_publications_policy.docx` section 1 (0.566); `who_publications_policy.pdf` p.13 (0.494) |
| What percentage of FY 2015 requests got a final response within 20 working days? | Y | `wb_access_to_information_fy2015.docx` section 1 (0.585); `wb_access_to_information_fy2015.docx` section 1 (0.541); `wb_access_to_information_fy2015.docx` section 1 (0.510) |
| What is the United Nations Security Management System accountability framework? | N | `un_policy_portal.txt` section 1 (0.640); `wb_ati_fy2015.pdf` p.11 (0.524); `wb_ati_fy2020.pdf` p.2 (0.497) |
| How do annual reports describe transparency initiatives and access to information statistics? | Y | `wb_ati_fy2011.pdf` p.3 (0.768); `wb_ati_fy2012.pdf` p.3 (0.765); `wb_ati_fy2013.pdf` p.3 (0.763) |

## Retrieved snippets

Snippets are trimmed for readability; the full returned snippets are in `search_results.jsonl`.

### 1. How does the World Bank proactively disclose information and make its document database publicly accessible?
- 1. `wb_ati_fy2022.pdf` (page 2), score 0.771: Introduction The World Bank’s Access to Information Policy, which was adopted in 2010, aims to promote transparency, accountability, and participation in the Bank’s operations and 
- 2. `wb_ati_fy2011.pdf` (page 32), score 0.769: 22 access to information / The World Bank Looking Forward In FY 2011, the World Bank took landmark steps to make information accessible by the public, and to globally promote trans
- 3. `wb_ati_fy2011.pdf` (page 26), score 0.765: 16 access to information / The World Bank The World Bank’s Exercise of its Prerogative to Disclose Restricted Information Under the AI Policy, the World Bank reserves the right, in
- Manual note: Top results are World Bank access-to-information reports with proactive-disclosure and public-access content.

### 2. How many information requests did the World Bank receive in FY 2015 and how quickly did it respond?
- 1. `wb_ati_fy2014.pdf` (page 18), score 0.789: 8 WORLD BANK / ACCESS TO INFORMATION ANNUAL REPORT FY 2014 Table 4 shows the public access requests completed by the World Bank in FY 2014 per quarterly period. Table 4. Public Req
- 2. `wb_ati_fy2015.pdf` (page 24), score 0.789: 12 WORLD BANK / ACCESS TO INFORMATION ANNUAL REPORT FY 2015 The 482 cases completed in FY 2015 include 456 cases received in FY 2015 and 26 cases carried over from preceding years:
- 3. `wb_access_to_information_fy2015.docx` (section 1), score 0.784: 1 from FY 2013, and 2 from FY 2012 (Table 6). Of the 482 AI requests completed in FY 2015, 326 were properly addressed to the World Bank and provided adequate information to enable
- Manual note: The FY2015 report appears at ranks 2 and 3; rank 1 is the adjacent FY2014 report, so fiscal-year precision is weak.

### 3. When can the World Bank disclose restricted information if the public benefit outweighs the potential harm?
- 1. `wb_ati_fy2015.pdf` (page 27), score 0.740: 15 REVIEW OF THE PUBLIC’S ACCESS TO INFORMATION IN FY 2015 • Required reopening the request because the requester modified the original request by adding additional documents to th
- 2. `wb_ati_fy2014.pdf` (page 22), score 0.697: 12 WORLD BANK / ACCESS TO INFORMATION ANNUAL REPORT FY 2014 Under the AI Policy, the World Bank reserves the right, in exceptional circumstances, to disclose information restricted
- 3. `wb_ati_fy2013.pdf` (page 12), score 0.696: 2 access to information / The World Bank Providing Information on Request In addition to the information that is readily accessible online, the World Bank responds directly to memb
- Manual note: The FY2014 result at rank 2 covers the restricted-information prerogative; rank 1 is a less direct FY2015 case page.

### 4. What appeals process is available for information requests denied under the Access to Information policy?
- 1. `wb_ati_fy2012.pdf` (page 52), score 0.774: 42 access to information / The World Bank 9. The AIC determined that the data requested does not already exist and is not readily available, and would need to be created or collate
- 2. `wb_ati_fy2012.pdf` (page 51), score 0.734: access to information (see paragraph 36 of the Policy). Based on the requester’s modification of the original request, the AIC finds that the Bank has not yet either considered or 
- 3. `wb_ati_fy2013.pdf` (page 43), score 0.717: ” exceptions under the AI Policy and, therefore, did not violate the AI Policy. For this reason, the AIC decided to uphold the Bank’s decision to deny public access to the Recordin
- Manual note: Appeals-related FY2012 and FY2013 pages are retrieved, though they are case outcomes rather than a concise process summary.

### 5. How does the World Bank use outreach, consultations and citizen engagement to improve transparency?
- 1. `wb_ati_fy2020.pdf` (page 2), score 0.725: ...................18 Over the past year, as countries have struggled with the COVID-19 pandemic, rising levels of unsustainable debt, growing inequality, and the urgent climate c
- 2. `wb_ati_fy2013.pdf` (page 18), score 0.722: 8 access to information / The World Bank Openness and Transparency As a part of the World Bank’s renewed strategic focus to eradicate extreme poverty and boost shared prosperity, t
- 3. `wb_ati_fy2013.pdf` (page 20), score 0.721: , moving data and documentation to the public sphere). Collaborating to Promote Open Development Solutions Supporting Governments in increasing transparency. The World Bank continu
- Manual note: FY2013 openness pages and the FY2020 report are related to outreach and transparency; wording is broad.

### 6. What strategy does WHO use to set publication priorities and monitor compliance with publishing policy?
- 1. `who_publications_policy.pdf` (page 56), score 0.661: Evaluation of the Impact of WHO Publications pre-publication version, October 2016 44 (e.g., effective WHO search engines, improved online publications directory and metadata, etc.
- 2. `who_publications_policy.pdf` (page 49), score 0.659: Evaluation of the Impact of WHO Publications pre-publication version, October 2016 37 to be cited, even after controlling for how long they had been available. It was not possible 
- 3. `who_publications_policy.docx` (section 1), score 0.657: of publications, use communications to increase awareness of publications such as notifications/alerts, sign-up distribution lists; and  Translation: translation into other langua
- Manual note: WHO publication-evaluation pages are retrieved, including strategic recommendations and publishing-policy discussion.

### 7. What role does the WHO Guidelines Review Committee play in developing guidelines?
- 1. `who_publications_policy.pdf` (page 50), score 0.613: made significant improvements in guideline development since its establishment in 2007 including the development and revision (2nd edition) of the WHO handbook for guideline develo
- 2. `who_publications_policy.docx` (section 1), score 0.566: , mostly free of charge. However, there has been no evaluation conducted of the relevance, quality, and impact of these publications. Various bodies have expressed a need for stren
- 3. `who_publications_policy.pdf` (page 13), score 0.494: methods, following the establishment of the GRC, were not even across the organization, while neither the GRC nor other quality standards were fully embedded across WHO.7 The 2012 
- Manual note: The WHO report contains guideline-development and Guidelines Review Committee passages.

### 8. What percentage of FY 2015 requests got a final response within 20 working days?
- 1. `wb_access_to_information_fy2015.docx` (section 1), score 0.585: 20 working days % of requests 16 Average working days to complete 81 For cases requiring more than 20 working days to complete in FY 2015, the average was 81 working days (Table 11
- 2. `wb_access_to_information_fy2015.docx` (section 1), score 0.541: 1 from FY 2013, and 2 from FY 2012 (Table 6). Of the 482 AI requests completed in FY 2015, 326 were properly addressed to the World Bank and provided adequate information to enable
- 3. `wb_access_to_information_fy2015.docx` (section 1), score 0.510: available to the public for direct searching and browsing in the Documents and Reports (D&R) database. In FY 2015, the public visited the D&R database more than 2.4 million times, 
- Manual note: The doc_type=docx filter returned only the FY2015 Word copy; its top result is the matching response-time table.

### 9. What is the United Nations Security Management System accountability framework?
- 1. `un_policy_portal.txt` (section 1), score 0.640: UN Policy Portal — source note The United Nations Policy Portal is an online compendium of the Secretariat regulatory framework. It includes policies and guidance for the Secretari
- 2. `wb_ati_fy2015.pdf` (page 11), score 0.524: OTHER WORLD BANK GROUP DISCLOSURE POLICIES International Finance Corporation: www.ifc.org/disclosure Multilateral Investment Guarantee Agency: www.miga.org/access Integrity Vice Pr
- 3. `wb_ati_fy2020.pdf` (page 2), score 0.497: ...................18 Over the past year, as countries have struggled with the COVID-19 pandemic, rising levels of unsustainable debt, growing inequality, and the urgent climate c
- Manual note: The top result is only a short UN Policy Portal source note. The actual Security Management System accountability framework is not in this corpus.

### 10. How do annual reports describe transparency initiatives and access to information statistics?
- 1. `wb_ati_fy2011.pdf` (page 3), score 0.768: access to information Annual Report FY 2011 Moving Forward Transparency and Accountability
- 2. `wb_ati_fy2012.pdf` (page 3), score 0.765: access to information Annual Report FY 2012 Moving Forward Transparency and Accountability
- 3. `wb_ati_fy2013.pdf` (page 3), score 0.763: access to information Annual Report FY 2013 Moving Forward Transparency and Accountability
- Manual note: The top results are consecutive World Bank Access to Information annual reports about transparency and statistics.

## Three weakest queries and improvements

1. **UN Security Management System accountability framework.** The corpus has only a short UN portal source note, so retrieval cannot answer the substantive question. Add the full UN Security Policy Manual and the framework chapter, with official source URLs.
2. **FY2015 request volume and response speed.** The top result is the neighboring FY2014 report, while the FY2015 report is ranks 2 and 3. Add fiscal year as a payload field and apply an exact year filter; a hybrid lexical/semantic reranker can also reward exact year terms.
3. **Access-to-information appeals process.** Results point to relevant appeal case examples, but not a concise description of the process. Add the primary policy and appeals-board procedures, and preserve headings in chunks so procedural passages rank above case decisions.

## Filter and threshold checks

- `filters={"doc_type": "docx"}` returned only DOCX results and retrieved the FY2015 response-time table.
- A query with `score_threshold=0.99` returned no results, confirming the optional similarity cutoff.
