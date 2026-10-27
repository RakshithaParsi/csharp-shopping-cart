# CampusMart — Simple Shopping Cart System (C# WinForms)

A modern, clean, and complete **Windows Forms (WinForms)** desktop application developed in **C#** and **.NET** for a college mini-project demonstration and viva presentation.

---

## 🌟 Key Features

1. **Modern Shopping Dashboard:**
   - Dark slate branding header with real-time cart summary badge (`Cart: X items | ₹X.XX`).
   - Dynamic search box and category filter dropdown ("Writing", "Paper & Notes", "Accessories", "Electronics").
   - Responsive product catalog built using modular product card controls.

2. **Accurate Sample Product Catalog (Indian Rupees ₹):**
   - 🖊️ **Pen** — ₹10.00
   - 📓 **Notebook** — ₹50.00
   - 🎒 **School Bag** — ₹500.00
   - 🍶 **Water Bottle** — ₹100.00
   - ✏️ **Pencil** — ₹5.00
   - 🧮 **Calculator** — ₹250.00

3. **Dynamic Shopping Cart:**
   - Real-time `DataGridView` table displaying Item Name, Unit Price, Quantity, Line Total.
   - **Duplicate Item Prevention:** Adding the same product increases its quantity rather than creating duplicate rows.
   - **Inline Quick Actions:** `[+]` increase, `[-]` decrease, and `[✕]` remove buttons directly in the cart grid.
   - Selected-row toolbar buttons for easy demonstration.

4. **Automatic Price Calculation:**
   - `Item Total = Unit Price × Quantity`
   - `Grand Total = Σ (All Item Totals)`
   - Subtotal and Grand Total update automatically via C# event handlers whenever any cart item is modified.

5. **Validation & Edge Case Handling:**
   - Prevents checkout with an empty shopping cart.
   - Validates student / customer name during checkout.
   - Decreasing an item's quantity to zero prompts or cleanly removes it from the cart.
   - Confirmation dialog before clearing the cart.

6. **Order Checkout & Receipt Confirmation:**
   - Order review dialog with customer details and simulated payment methods (Cash, Campus Card, UPI Simulator).
   - Generates an itemized tax invoice receipt with Order ID, timestamp, items table, and final grand total.
   - Option to copy receipt to clipboard and automatically resets the cart for the next customer.

---

## 📁 Project Architecture & Structure

```
SimpleShoppingCart/
│
├── SimpleShoppingCart.sln          # Visual Studio Solution file
├── SimpleShoppingCart.csproj       # Main WinForms Project Configuration
├── Program.cs                      # Application Entry Point ([STAThread] Main)
│
├── Models/                         # Data Models
│   ├── Product.cs                  # Product entity (Id, Name, Price, Category, etc.)
│   ├── CartItem.cs                 # Cart item with calculations (UnitPrice, TotalPrice)
│   └── Order.cs                    # Completed order entity & tax invoice receipt generator
│
├── Services/                       # Business Logic Layer
│   ├── CartManager.cs              # In-memory cart manager, events, and totals calculation
│   └── ProductRepository.cs        # In-memory catalog repository with search & filtering
│
├── UI/                             # User Interface (WinForms)
│   ├── MainForm.cs                 # Main dashboard with Catalog & Cart split view
│   ├── ProductCardControl.cs       # Reusable card component for catalog products
│   ├── CheckoutForm.cs             # Modal dialog for customer details & order confirmation
│   └── OrderReceiptForm.cs         # Post-checkout invoice receipt & copy screen
│
├── Utils/
│   └── ImageHelper.cs              # GDI+ vector-style product icon badge generator
│
└── Tests/                          # Automated Unit Test Suite
    ├── SimpleShoppingCart.Tests.csproj
    └── ShoppingCartTests.cs        # 12 xUnit tests verifying business rules & math
```

---

## 🚀 How to Run the Application

### Option A: Using Visual Studio 2022 (Recommended for Viva / College)
1. Open **Visual Studio 2022**.
2. Click **Open a project or solution**.
3. Select `SimpleShoppingCart.sln` inside the project folder.
4. Set the startup project to **SimpleShoppingCart** (if not already set).
5. Press **F5** or click the green **▶ Start** button.

### Option B: Using .NET CLI (Command Prompt or PowerShell)
1. Open PowerShell or Command Prompt.
2. Navigate to the project directory:
   ```powershell
   cd "C:\Users\My Files\.gemini\antigravity\scratch\SimpleShoppingCart"
   ```
3. Run the application:
   ```powershell
   dotnet run
   ```

### Option C: Running the Compiled Executable Directly
You can run the built `.exe` without opening an IDE:
```powershell
.\bin\Debug\net10.0-windows\SimpleShoppingCart.exe
```

---

## 🧪 Running the Automated Unit Tests

To run the automated test suite verifying all calculations:
```powershell
dotnet test
```
**Results:** 12 passed, 0 failed.

---

## 🎓 College Mini-Project Viva & Presentation Guide

| Viva Question | Model Answer |
| :--- | :--- |
| **Q1: What architecture does this application follow?** | The project follows a **Layered Architecture** with clear separation of concerns: **Models** (data structures), **Services** (business rules and in-memory repository), and **UI** (WinForms presentation layer). |
| **Q2: How are cart items stored?** | Cart items are stored using an in-memory `List<CartItem>` managed by the `CartManager` class. |
| **Q3: How does the application avoid duplicate rows when the same item is added?** | In `CartManager.AddProduct()`, the system searches the cart using LINQ `_items.FirstOrDefault(i => i.ProductId == product.Id)`. If found, it invokes `existingItem.Increment(quantity)` instead of adding a new entry. |
| **Q4: How does the UI update automatically when items change?** | `CartManager` exposes a custom C# event: `event EventHandler<CartChangedEventArgs> CartChanged`. Whenever an item is added, incremented, decremented, or removed, this event fires and `MainForm` invokes `RefreshCartDisplay()` to refresh the table and totals. |
| **Q5: How is the grand total calculated?** | `GrandTotal` is computed using LINQ aggregation: `_items.Sum(i => i.TotalPrice)`, where `TotalPrice = UnitPrice * Quantity` for each cart item. |
| **Q6: Why is in-memory storage used instead of a database?** | As specified for a mini-project prototype, in-memory collections provide high-performance, zero-configuration setup without requiring SQL Server or external database drivers on the evaluation machine. |
