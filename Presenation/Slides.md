---
marp: true
theme: default
paginate: true
size: 16:9
html: true
style: |
  /* GLOBAL */

  section {
    font-family: "Aptos", "Inter", "Arial", sans-serif;
    background: #FFFFFF;
    color: #1F2937;
    padding: 55px 70px;
    font-size: 25px;
  }

  /* HEADINGS */

  h1 {
    color: #14532D;
    font-size: 42px;
    font-weight: 700;
    margin-bottom: 20px;
  }

  h2 {
    color: #14532D;
    font-size: 34px;
    font-weight: 650;
    border-bottom: 3px solid #DCFCE7;
    padding-bottom: 10px;
  }

  h3 {
    color: #166534;
    font-size: 27px;
  }

  strong {
    color: #166534;
  }

  /* TEXT */

  p {
    line-height: 1.45;
  }

  ul {
    line-height: 1.45;
  }

  li::marker {
    color: #16A34A;
  }

  em {
    color: #64748B;
  }

  /* CODE  */

  code {
    background: #F1F5F9;
    color: #14532D;
    padding: 3px 7px;
    border-radius: 4px;
  }

  /*  TABLES */

  table {
    font-size: 21px;
  }

  th {
    background: #14532D;
    color: #FFFFFF;
  }

  td {
    border-color: #E2E8F0;
  }

  /* BLOCKQUOTE  */

  blockquote {
    border-left: 5px solid #16A34A;
    background: #F0FDF4;
    padding: 15px 20px;
    color: #166534;
  }

  /* BOXES */

  .box {
    background: #F0FDF4;
    border-left: 5px solid #16A34A;
    border-radius: 8px;
    padding: 18px 24px;
    margin: 15px 0;
  }

  .box-title {
    color: #14532D;
    font-weight: 700;
    font-size: 25px;
    margin-bottom: 8px;
  }

  .box-note {
    background: #EFF6FF;
    border-left: 5px solid #2563EB;
    border-radius: 8px;
    padding: 18px 24px;
    margin: 15px 0;
  }

  .box-note .box-title {
    color: #1D4ED8;
    font-weight: 700;
    font-size: 25px;
    margin-bottom: 8px;
  }


  /* Yellow - Warning */
  .box-warning {
    background: #FFFBEB;
    border-left: 5px solid #F59E0B;
    border-radius: 8px;
    padding: 18px 24px;
    margin: 15px 0;
  }

  .box-warning .box-title {
    color: #B45309;
    font-weight: 700;
    font-size: 25px;
    margin-bottom: 8px;
  }


  /* Red - Danger / Important Issue */
  .box-danger {
    background: #FEF2F2;
    border-left: 5px solid #DC2626;
    border-radius: 8px;
    padding: 18px 24px;
    margin: 15px 0;
  }

  .box-danger .box-title {
    color: #B91C1C;
    font-weight: 700;
    font-size: 25px;
    margin-bottom: 8px;
  }

  /* HIGHLIGHTS */

  .highlight {
    background: #DCFCE7;
    color: #14532D;
    padding: 3px 8px;
    border-radius: 5px;
    font-weight: 600;
  }

  .muted {
    color: #64748B;
    font-weight: 700;
  }

  .small {
    font-size: 19px;
  }

  /* TWO COLUMN LAYOUT */

  .two-col {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 35px;
  }

  /* TITLE SLIDE */

  section.lead {
    border-left: 12px solid #14532D;
    display: flex;
    flex-direction: column;
    justify-content: center;
  }

  section.lead h1 {
    font-size: 48px;
    line-height: 1.15;
  }

  /* SECTION DIVIDER */

  section.section {
    background: #14532D;
    color: #FFFFFF;
    display: flex;
    flex-direction: column;
    justify-content: center;
  }

  section.section h1 {
    color: #86EFAC;
    font-size: 70px;
    margin-bottom: 5px;
  }

  section.section h2 {
    color: #FFFFFF;
    border: none;
    font-size: 42px;
  }

  section.section .muted {
    color: #DCFCE7;
  }

  /* HEADER / FOOTER */

  header {
    color: #64748B;
  }

  footer {
    color: #64748B;
  }
---

<!-- SECTION -->
<!-- _class: section -->

# Paper Presentation

## SHAP-based Explanations are Sensitive to Feature Representation

<br>

<span class="muted">
M Sreenath Karthick (112301042)
</span>
<span class="muted">
M Murali Karthick (112301019)
</span>

---