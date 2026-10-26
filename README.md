# Cashew MCP

**Give your agent Singapore's mortgage brain.**

The Cashew MCP exposes our live rate engine as callable tools, the same engine behind [cashew.sg](https://www.cashew.sg). Rank real bank packages, run stamp-duty and affordability maths, and model restructuring plays, all from inside your own agent.

Every figure is computed against bank rates synced daily, the same numbers Cashew shows its own customers. All major Singapore banks. HDB, private and commercial. Fixed and floating (SORA).

## What it does

- **Find the best loan packages.** Rank live packages from all major Singapore banks for any borrower profile, sorted by the real first-year rate.
- **Calculate the mortgage maths.** Monthly repayment, total interest, affordability and the income a target loan needs, returned in a single call.
- **Run any scenario.** From refinancing savings to every IRAS stamp duty, the maths is built in.
- **Plan the bigger moves.** Decoupling and sell-one-buy-two, costed end to end with valuations on tap.

## Connect

Add the server to Claude Desktop, Cursor, or any MCP-compatible client. There's no key to paste.

```json
{
  "mcpServers": {
    "cashew": {
      "url": "https://www.cashew.sg/mcp"
    }
  }
}
```

Using Claude Code? Install it as a plugin instead:

```
claude plugin marketplace add cashew-sg/cashew-mcp
claude plugin install cashew-mcp@cashew
```

Then ask your agent something like: *"Should I refinance my $640k OCBC loan at 3.1%? I have 22 years left."*

Two tools, `recommend_packages` and `get_property_valuation`, ask you to sign in with your Cashew account the first time you use them. Everything else works straight away.

## Tools

**Mortgage and loan maths.** Turn a borrower profile into ranked packages and hard numbers.

- `recommend_packages`: rank live Singapore mortgage packages for a given borrower profile.
- `calculate_mortgage_repayment`: monthly repayment, total interest and total cost for a hypothetical loan.
- `calculate_package_schedule`: full amortisation schedule for a specific package using its actual rate path.
- `calculate_affordability`: max loan and property price from income and existing commitments.
- `calculate_required_income`: the reverse, income needed to support a target loan amount.
- `calculate_refinance_savings`: compare an existing mortgage against a new rate for monthly and lifetime savings.

**Stamp duties.** Every IRAS duty a Singapore transaction can attract.

- `calculate_residential_stamp_duty`: BSD and ABSD for a given buyer profile.
- `calculate_seller_stamp_duty`: SSD based on the holding period of the property.
- `calculate_mortgage_stamp_duty`: duty payable on the loan facility itself.
- `calculate_rental_stamp_duty`: duty on a lease, from rent and tenure.

**Restructuring and investment analysis.** Model the bigger property plays end to end.

- `analyze_decoupling`: cost versus benefit of transferring one co-owner's share.
- `analyze_sell_one_buy_two`: costs, loan sizing and stamp duty for the sell-one-buy-two play.
- `get_property_valuation`: indicative valuation for a specific unit.

**Reference data.** The raw lookups the maths is built on.

- `list_banks`: active banks on the platform.
- `list_market_indices`: benchmark indices such as SORA.
- `get_index_quotes`: historical rate-by-date for a given index.

Full details and a live playground: https://www.cashew.sg/intro-to-mcp

## Good to know

- Amounts are in Singapore dollars.
- Free while in beta. Building something with it? Tell us at support@cashew.sg.
- [Privacy](https://www.cashew.sg/privacy) · [Terms](https://www.cashew.sg/terms)
