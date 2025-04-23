# features/ Directory Specification

The `features/` directory contains Gherkin feature specifications that define tests for the component. These files use the Gherkin v6 syntax to describe behavior in a human-readable format.

## Purpose

- Define expected behavior of the component
- Provide test scenarios for implementation
- Document use cases in a structured format
- Enable behavior-driven development (BDD)
- Specify acceptance criteria for component implementations
- Facilitate communication between stakeholders

## User Story Mapping and Gherkin

User Story Mapping is a powerful technique that pairs exceptionally well with Gherkin specifications. In this approach:

- Each **Feature file** represents a single **use case** or user story
- Each **Rule** within a feature represents a specific **business rule** for that use case
- **Examples** (scenarios) demonstrate how the business rules apply in concrete situations

This organization creates a natural mapping between:
1. The user's journey through your system (represented by features)
2. The business rules that govern that journey (represented by rules)
3. Concrete examples that illustrate those rules (represented by examples/scenarios)

By structuring your Gherkin files according to User Story Mapping principles, you create specifications that are more intuitive, easier to navigate, and better aligned with how users actually experience your system.

## Gherkin v6 Structure

Each `.feature` file in the directory follows the Gherkin v6 syntax with:

### Feature

A high-level description of a feature with:

- A title
- A description (often in the form of a user story: As a [role], I want [feature], So that [benefit])
- Optional tags for categorization

### Rule

The `Rule:` keyword groups related examples that illustrate a specific business rule. Each feature can contain multiple rules, and each rule can contain multiple examples.

### Background

The `Background:` section defines steps that are common to all examples in a feature or rule. Background steps run before each example.

### Examples (Scenarios)

In Gherkin v6, the terms "Example" and "Scenario" are synonymous, but "Example" is preferred as it better conveys that these are concrete examples of the rules in action.

Each example includes:
- A descriptive title
- One or more steps using the Given/When/Then format

### Steps

Steps are the building blocks of examples and use the Given/When/Then format:

- **Given** steps set up the initial context (preconditions)
- **When** steps describe the action being taken
- **Then** steps describe the expected outcome
- **And/But** steps provide additional context, actions, or expectations

## Complete Example with User Story Mapping

```gherkin
@user-management @authentication
Feature: User Registration
  As a new user
  I want to create an account
  So that I can access the system

  Background:
    Given the registration page is open
    And the following validation rules are configured:
      | Email | Password                         |
      | Must be a valid email format | Minimum 8 characters with 1 number |

  Rule: New users can register with valid information

    Example: Successful registration with valid details
      When I enter valid registration details
        | Email           | Password       | FirstName | LastName |
        | user@example.com| SecureP@ssw0rd | John      | Doe      |
      And I submit the registration form
      Then a new user account should be created
      And I should receive a verification email
      And I should be redirected to the login page

    Example: Registration with optional information
      When I enter valid registration details
        | Email           | Password       | FirstName | LastName |
        | user@example.com| SecureP@ssw0rd | John      | Doe      |
      And I provide optional profile information
        | Phone           | Address        |
        | +1-555-123-4567 | 123 Main St    |
      And I submit the registration form
      Then a new user account should be created with the optional information
      And I should receive a verification email

  Rule: Users cannot register with invalid information

    Example: Registration with existing email
      Given a user with email "existing@example.com" already exists
      When I enter registration details with email "existing@example.com"
      And I submit the registration form
      Then I should see an error message "Email already in use"
      And no new account should be created

    Example Outline: Registration with invalid input
      When I enter registration details
        | Email    | Password   | FirstName   | LastName   |
        | <email>  | <password> | <firstName> | <lastName> |
      And I submit the registration form
      Then I should see an error message "<errorMessage>"
      And no new account should be created

      Examples:
        | email           | password    | firstName | lastName | errorMessage                   |
        | invalid-email   | Password1   | John      | Doe      | Invalid email format           |
        | user@example.com| short       | John      | Doe      | Password too short             |
        | user@example.com| Password1   |           | Doe      | First name is required         |
        | user@example.com| Password1   | John      |          | Last name is required          |
```

## Step Parameters

Steps can include parameters to make them more reusable:

### String Parameters

Quoted strings for text values:

```gherkin
Given the product "Organic Apples" is in the catalog
When the customer searches for "Apple"
```

### Numeric Parameters

Numbers without quotes:

```gherkin
Given the product has a price of 10.99
When the customer orders 5 units
```

### Table Parameters

Tables for structured data:

```gherkin
Given the catalog contains the following products:
  | Product | Price | Category |
  | Apple   | 0.99  | Fruit    |
  | Bread   | 2.49  | Bakery   |
  | Milk    | 1.99  | Dairy    |
```

### Doc String Parameters

Doc strings for multi-line text:

```gherkin
Given the product description is:
  """
  Organic, locally-grown apples.
  Available in Red Delicious, Gala, and Granny Smith varieties.
  Perfect for baking or eating fresh.
  """
```

## Tags

Tags help categorize and filter examples:

```gherkin
@inventory @critical
Feature: Inventory Management

  @happy-path
  Example: Update inventory after sale
    # ...

  @error-handling
  Example: Handle insufficient inventory
    # ...
```

Common tag uses:
- Feature categories: `@inventory`, `@shipping`, `@reporting`
- Importance: `@critical`, `@high`, `@low`
- Status: `@wip` (work in progress), `@blocked`, `@deprecated`
- Behavior type: `@happy-path`, `@error-handling`, `@edge-case`

## Best Practices for Writing Feature Files

1. **Focus on behavior, not implementation**: Describe what the component should do, not how it should do it.
   ```gherkin
   # Good - focuses on business behavior
   When the customer adds a product to the cart
   Then the cart total should reflect the product price

   # Not as good - focuses on implementation
   When the addToCart() method is called
   Then the cartTotal property should be updated
   ```

2. **Use domain language**: Write scenarios using terminology that stakeholders understand.
   ```gherkin
   # Good - uses domain language
   Given the product is out of stock
   When a customer attempts to order the product
   Then the order should be rejected with "Out of Stock" reason

   # Not as good - uses technical language
   Given the product.stockLevel is 0
   When orderService.placeOrder() is called
   Then an OutOfStockException should be thrown
   ```

3. **Keep examples independent**: Each example should be able to run independently of others.
4. **Be specific**: Provide concrete examples with actual values.
5. **Cover edge cases**: Include examples for error conditions and unusual situations.
6. **Use tables for multiple inputs**: When testing with various inputs, use Gherkin tables for clarity.
7. **Maintain consistency**: Use consistent language patterns across all feature files.
8. **Balance detail level**: Include enough detail to be clear, but not so much that the examples become brittle.

## Organizing Feature Files with User Story Mapping

When organizing feature files according to User Story Mapping principles, structure them by user activities and journeys:

```
features/
  customer_journey/
    browse_products.feature
    add_to_cart.feature
    checkout.feature
    track_order.feature
  store_management/
    manage_inventory.feature
    process_returns.feature
    generate_reports.feature
  admin_activities/
    manage_users.feature
    configure_system.feature
    audit_activities.feature
```

This organization:
1. Groups features by user roles and activities
2. Follows the natural flow of user journeys
3. Makes it easier to understand the system from a user's perspective
4. Helps identify gaps in your specifications

Each feature file should represent a complete user story or use case, with rules representing the business rules that apply to that use case.

## Recommended Reading

- **"The BDD Books - Formulation Document Examples with Given/When/Then"** by Gáspár Nagy and Seb Rose (2021) - A comprehensive guide to writing effective Gherkin specifications
- **"User Story Mapping: Discover the Whole Story, Build the Right Product"** by Jeff Patton - Essential reading for understanding how to organize requirements effectively