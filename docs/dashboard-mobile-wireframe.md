# Dashboard Mobile Wireframe (Mid-Fidelity)

## 1) Screen intent
**Screen:** Dashboard  
**Audience:** Students + early working professionals  
**Primary job-to-be-done:** Help users quickly understand what was auto-tracked, what needs review, and how much budget runway remains.

---

## 2) Layout structure (mobile, portrait)
**Viewport reference:** 390 x 844 pt (iPhone 13 baseline)  
**Safe-area aware:** Top notch + bottom home indicator

### Vertical layout map (top to bottom)
1. **Top App Bar (56 pt + safe top inset)**
   - Left: profile/avatar entry point
   - Center: “Dashboard”
   - Right: notifications icon + unread dot

2. **Monthly Snapshot Card (136 pt)**
   - Month selector (e.g., “April 2026 ▾”)
   - Total spent (large number)
   - Budget remaining progress bar
   - Microcopy: “Auto-tracked 92% of transactions”

3. **Needs Review Strip (dynamic height, min 72 pt)**
   - Horizontal chips/cards for uncategorized or low-confidence items
   - CTA: “Review now”

4. **Spending by Category Block (220 pt)**
   - Donut chart + legend
   - Toggle: `This month | Last month`

5. **Recent Auto-Tracked Transactions List (flex, scrollable)**
   - Row structure: merchant icon, merchant + category, amount, confidence badge
   - Inline swipe actions (edit category, mark personal/shared)

6. **Sticky Bottom Nav (64 pt + safe bottom inset)**
   - Tabs: Home, Transactions, Insights, Profile
   - Active tab: Home

---

## 3) Component grouping

## A. Global chrome
- **App bar**
- **Bottom navigation**

### B. Financial status cluster (top priority)
- Monthly Snapshot Card
  - Budget metric group
  - Progress indicator
  - Auto-tracking confidence summary

### C. Exception handling cluster
- Needs Review Strip
  - “Uncategorized (3)” chip
  - “Duplicate suspected (1)” chip
  - “Subscription changed amount (1)” chip

### D. Analysis cluster
- Spending by Category
  - Chart module
  - Comparative toggle

### E. Evidence cluster
- Recent transactions
  - Filter row (All / Auto / Manual)
  - List rows with confidence signal

---

## 4) Spacing logic (8pt grid)

### Global spacing tokens
- `xs = 8`
- `sm = 16`
- `md = 24`
- `lg = 32`

### Applied spacing rules
- Screen horizontal padding: **16 pt**
- Inter-section vertical spacing: **16 pt**
- Card internal padding: **16 pt**
- Title-to-subtitle spacing: **8 pt**
- Icon-to-label spacing: **8 pt**
- List row height: **64 pt** (8pt-grid aligned)
- Chip height: **32 pt**
- Touch targets: **min 44 x 44 pt**

### Rhythm examples
- App bar bottom to snapshot top: `16`
- Snapshot to review strip: `16`
- Review strip to category chart: `16`
- Chart to transaction list header: `24` (to denote major section change)

---

## 5) Interaction states

### A. Monthly Snapshot Card
- **Default:** Shows current month spend + remaining budget
- **Pressed:** 2% darker background, subtle scale to 0.98
- **Expanded (tap month):** Bottom sheet with month picker
- **Loading:** Skeleton amount + animated progress placeholder

### B. Needs Review Strip
- **Default:** Shows top 1–3 issue chips
- **Overflow:** “+N more” chip opens full review queue
- **Resolved item:** Chip fades out and count decrements with toast “Updated”

### C. Category chart
- **Default:** Donut + top 5 categories
- **Tap category segment:** Highlights segment, filters transaction list below
- **No data:** Empty-state illustration with text “No spending data yet”

### D. Transaction rows
- **Default row:** Merchant, date, amount, confidence badge (High/Medium/Low)
- **Swipe left:** Reveal `Edit category`, `Split`, `Hide`
- **Tap row:** Open transaction detail drawer
- **Long press:** Multi-select mode for bulk recategorization

### E. Bottom nav
- **Active tab:** Filled icon + label emphasis
- **Inactive tab:** Outline icon + muted label
- **Badge:** Red dot for actionable reviews if >0

---

## 6) Edge cases and behavior

1. **No linked accounts**
   - Replace snapshot card with onboarding CTA:
     - “Connect your bank to start automatic tracking”
     - Primary button: “Connect account”

2. **Sync delayed / bank API outage**
   - Inline warning banner under app bar:
     - “We’re updating your transactions. Last synced 6h ago.”
   - Keep previous data visible (stale-but-usable)

3. **All transactions uncategorized**
   - Needs Review becomes first major block
   - Chart hidden; show suggestion module for category rules

4. **Negative remaining budget**
   - Progress bar switches to critical color
   - Microcopy: “You are $120 over budget”
   - Offer quick action: “Find top overspending areas”

5. **Very low confidence auto-matching**
   - Confidence badge “Low” shown prominently
   - Review strip prioritizes these items at top

6. **First day of month with sparse data**
   - Snapshot includes forecast hint:
     - “Projected spend based on last 3 months”

7. **High transaction volume (100+)**
   - Transactions list paginates/infinite scroll
   - Sticky mini-filter appears after first 8 rows

8. **Accessibility edge cases**
   - Dynamic text up to 200% without clipping
   - Color contrast AA minimum
   - All key actions reachable via screen reader labels

---

## 7) Structured wireframe blueprint (implementation-ready)

```yaml
screen: dashboard_mobile
grid: 8pt
viewport:
  width: 390
  height: 844
regions:
  - id: app_bar
    height: 56
    padding_x: 16
    items: [avatar_button, title_dashboard, notification_button]

  - id: monthly_snapshot_card
    margin_top: 16
    height: 136
    padding: 16
    items:
      - month_selector
      - total_spent_value
      - budget_remaining_progress
      - auto_tracking_confidence_text

  - id: needs_review_strip
    margin_top: 16
    min_height: 72
    items:
      - review_chip_uncategorized
      - review_chip_duplicates
      - review_chip_subscription_change
      - review_now_cta

  - id: category_block
    margin_top: 16
    height: 220
    items:
      - timeframe_toggle
      - donut_chart
      - legend_top_categories

  - id: transactions_section
    margin_top: 24
    items:
      - filter_tabs
      - transaction_list_rows

  - id: bottom_nav
    position: sticky_bottom
    height: 64
    items: [tab_home, tab_transactions, tab_insights, tab_profile]
interaction_priority:
  primary: [review_uncategorized, inspect_budget_remaining, edit_transaction_category]
  secondary: [switch_timeframe, filter_transactions]
states:
  loading: skeleton_cards_and_rows
  empty: connect_account_or_no_transactions
  error: sync_delay_banner_with_retry
```
