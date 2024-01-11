---
layout: page
title: "#004 : Weather Forecast"
cover-img: /assets/img/project-004/ilustrasi-cuaca-kupang-ntt.jpeg
---

In this project, I want to implement not only advanced spreadsheet formulas, but also [Google App Scripts](https://www.google.com/script/start/) in data operations. In simple terms, it is a tool that allows you to write code based on JavaScript that interacts with Google Workspace applications like Google Sheets, Google Docs, and Google Forms. One of many use case to implement Google App Script is making a form, directly in a Google Sheet file. Let's say that I have an Indonesian-traditional cake store and I want to make the order form goes like this.

<video width="720" controls>
  <source src="/assets/img/project-003/GCHnnz-bsAAW76K_vid.mp4" type="video/mp4">
</video>

This is the flow of using this form:
1. An admin input customer orders from "Order Form" ("Form Pemesanan") sheets. They must fills the customer information (maybe we want to make CRM system later), the delivery method, the delivery date, and the order details. I add data validation and ```XLOOKUP``` formula so the admin can easily select the menu and when it's done, it shows up the image and the price immediately.
2. If the admin is incovenient when selecting the menu one-by-one, they can press "Tampilkan Semua Menu" button to show up all menu, so they only input the quantity of order from each menu.
3. After completing all necessary fields, they can send the form to the _"database"_ using "Masukkan Form" button.
4. When the admin want to input a new order, they can reset the table by pressing "Reset Tabel" button.

When the form is sent, they immediately goes into "Data Pemesanan" sheet. This is just like a raw data that freshly extracted from a source, so I "normalize" the data by cleaning it and normalized it.

![Data Pemesanan aka "Order Details"](/assets/img/project-003/GCHoO4jacAAzYSB.jpeg)

"Data Pemesanan" or "Order Details" sheet can be normalized into "Transactions", "Customer", and "Menu". The "Menu" table is independent from the user input, so I have already prepared it before any orders enter.

![Menu table](/assets/img/project-003/GCHpuRaa0AAY-fA.jpeg)

I also add "Void" sheet with "Void" button and _filter view_ to allow admin makes some orders invalid. This is necessary because there are human errors and "test" orders. 

![Void feature](/assets/img/project-003/GCHppKxbkAA1giP.jpeg)

By the way, why do I bother to do all of this stuff when Google Form exists? There are several reasons:
1. Google Form has some limitations when it comes to customizing the appearance of a form. For instance, the layout or buttons may not be as flexible as one would like.
2. User convenience. Perhaps the admin who inputs the form wants to immediately know the order results, they can simply scroll or switch sheets without having to switch between applications.

Overall, I think an order form like this can be reliable for small businesses and home-based businesses that don’t require many advanced features and prioritize the ease of use. However, it should be modified for data security.  

If you want to check the script and the spreadsheet file, you can copy it from this [link](https://docs.google.com/spreadsheets/d/1FPEgTdrLNy4CKX8Otn0NrdKs6UIPYQs4QXCALHydr74/copy). Cheers!