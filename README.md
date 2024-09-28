<div align="center"> <br /> <a href="https://youtu.be/PuOVqP_cjkE?feature=shared" target="_blank"> <img src="https://github.com/ogbuigbo/banking/assets/151519281/3c03519c-7ebd-4539-b598-49e63d1770b4" alt="Project Banner"> </a> <br /> <div> <img src="https://img.shields.io/badge/-Next_JS-black?style=for-the-badge&logoColor=white&logo=nextdotjs&color=000000" alt="nextdotjs" /> <img src="https://img.shields.io/badge/-TypeScript-black?style=for-the-badge&logoColor=white&logo=typescript&color=3178C6" alt="typescript" /> <img src="https://img.shields.io/badge/-Tailwind_CSS-black?style=for-the-badge&logoColor=white&logo=tailwindcss&color=06B6D4" alt="tailwindcss" /> <img src="https://img.shields.io/badge/-Appwrite-black?style=for-the-badge&logoColor=white&logo=appwrite&color=FD366E" alt="appwrite" /> </div> <h3 align="center">Fintech Bank Application</h3> </div>

Table of Contents
🤖 Introduction
⚙️ Tech Stack
🔋 Features
🤸 Quick Start
🕸️ Code Snippets
🔗 Assets
🚀 More
🤖 Introduction
This Fintech Bank Application, built with Next.js, offers a comprehensive platform for users to manage their finances seamlessly. With real-time transaction data, secure authentication, and multi-bank account integration, users can keep track of their accounts, transfer money, and monitor their spending habits—all in one place. It provides a modern, scalable solution tailored for personal finance management.

Connect with a community of developers and enthusiasts in our active Discord channel, boasting over 34k+ members.

<a href="https://discord.com/invite/n6EdbFJ" target="_blank"><img src="https://github.com/sujatagunale/EasyRead/assets/151519281/618f4872-1e10-42da-8213-1d69e486d02e" /></a>

⚙️ Tech Stack
Next.js: A powerful framework for building modern web applications.
TypeScript: Enhancing JavaScript with types for more robust development.
Appwrite: A self-hosted backend-as-a-service platform to handle server functions.
Plaid: Integration for linking multiple bank accounts securely.
Dwolla: Payment solution for transferring funds.
React Hook Form: For flexible and easy form handling.
Zod: Data validation for schemas.
TailwindCSS: A utility-first CSS framework.
Chart.js: Visual representation of data through dynamic charts.
ShadCN: Component library for building responsive designs.
🔋 Features
Secure Authentication: Featuring server-side rendering (SSR) for added security, with proper validation and authorization methods.
Bank Connectivity: Integrates with Plaid, allowing users to link multiple bank accounts and view real-time data.
Account Overview: Displays an overview of total balances, recent transactions, and categorized spending insights.
Transaction History: Filter and paginate through detailed transaction records.
Fund Transfers: Enables seamless money transfers between users using Dwolla.
Real-time Updates: Synchronizes new bank data across all pages instantly.
Responsive Design: Optimized for desktop, tablet, and mobile devices.
🤸 Quick Start
Set up this project locally by following the steps below:

Prerequisites
Ensure the following are installed on your machine:

Git
Node.js
npm
Installation
Clone the repository and install dependencies:
git clone https://github.com/ogbuigbo/banking.git
cd banking
npm install

Environment Setup
Create a .env file in the root directory and fill in your credentials:
NEXT_PUBLIC_SITE_URL=
NEXT_PUBLIC_APPWRITE_ENDPOINT=
APPWRITE_DATABASE_ID=
...
PLAID_CLIENT_ID=
PLAID_SECRET=
...
DWOLLA_KEY=
DWOLLA_SECRET=
Run the Project
bash
Copy code
npm run dev

exchangePublicToken
export const exchangePublicToken = async ({ publicToken, user }) => {
  try {
    const response = await plaidClient.itemPublicTokenExchange({
      public_token: publicToken,
    });
    const accessToken = response.data.access_token;
    const accountData = (await plaidClient.accountsGet({ access_token: accessToken })).data.accounts[0];
    const processorTokenResponse = await plaidClient.processorTokenCreate({
      access_token: accessToken,
      account_id: accountData.account_id,
      processor: "dwolla",
    });
    const processorToken = processorTokenResponse.data.processor_token;
    const fundingSourceUrl = await addFundingSource({ dwollaCustomerId: user.dwollaCustomerId, processorToken, bankName: accountData.name });
    if (!fundingSourceUrl) throw Error;
    await createBankAccount({
      userId: user.$id,
      bankId: itemId,
      accountId: accountData.account_id,
      accessToken,
      fundingSourceUrl,
      sharableId: encryptId(accountData.account_id),
    });
    revalidatePath("/");
    return { publicTokenExchange: "complete" };
  } catch (error) {
    console.error("Error creating token:", error);
  }
};
