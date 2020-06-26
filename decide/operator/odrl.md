# `ODRL` Binary Operators

Prefix: `odrl`. 

See also:

- [ODRL Information Model 2.2](https://www.w3.org/TR/odrl-model/)
- [IPTC RightsML Standard 2.0](https://iptc.org/std/RightsML/2.0/RightsML_2.0-specification.html)


## Constraint Operators

[constraintRelationalOperators](https://www.w3.org/TR/odrl-vocab/#constraintRelationalOperators)

### [odrl:eq](http://www.w3.org/ns/odrl/2/eq)
Indicating that a given value equals the right operand of
 the Constraint.

### [odrl:gt](http://www.w3.org/ns/odrl/2/gt)
Indicating that a given value is greater than the right operand of
 the Constraint.

### [odrl:gteq](http://www.w3.org/ns/odrl/2/gteq)
Indicating that a given value is greater than or equal to the right
 operand of the Constraint.

### [odrl:hasPart](http://www.w3.org/ns/odrl/2/hasPart)
A set-based operator indicating that a given value contains the right
 operand of the Constraint.

### [odrl:isA](http://www.w3.org/ns/odrl/2/isA)
A set-based operator indicating that a given value is an instance of
 the right operand of the Constraint.

### [odrl:isAllOf](http://www.w3.org/ns/odrl/2/isAllOf)
A set-based operator indicating that a given value is all of the
 right operand of the Constraint.

### [odrl:isAnyOf](http://www.w3.org/ns/odrl/2/isAnyOf)
A set-based operator indicating that a given value is any of the
 right operand of the Constraint.

### [odrl:isNoneOf](http://www.w3.org/ns/odrl/2/isNoneOf)
A set-based operator indicating that a given value is none of the
 right operand of the Constraint.

### [odrl:isPartOf](http://www.w3.org/ns/odrl/2/isPartOf)
A set-based operator indicating that a given value is contained by the
 right operand of the Constraint.

### [odrl:lt](http://www.w3.org/ns/odrl/2/lt)
Indicating that a given value is less than the right operand of the
 Constraint.

### [odrl:lteq](http://www.w3.org/ns/odrl/2/lteq)
Indicating that a given value is less than or equal to the right
 operand of the Constraint.

### [odrl:neq](http://www.w3.org/ns/odrl/2/neq)
Indicating that a given value is not equal to the right operand of
 the Constraint.


## Logical Constraint Operands

- [term-LogicalConstraint](https://www.w3.org/TR/odrl-vocab/#term-LogicalConstraint)
- [constraintLogicalOperands](https://www.w3.org/TR/odrl-vocab/#constraintLogicalOperands)

> This property *MUST* only be used for Logical Constraints, and the list
 of operand values *MUST* be Constraint instances.

### [odrl:and](http://www.w3.org/ns/odrl/2/and)

The relation is satisfied when all of the Constraints are satisfied.

### [odrl:andSequence](http://www.w3.org/ns/odrl/2/andSequence)

The relation is satisfied when each of the Constraints are satisfied in
 the order specified.

- [**odrl:andSequence**](https://www.w3.org/TR/odrl-vocab/#term-andSequence)

### [odrl:or](http://www.w3.org/ns/odrl/2/or)

The relation is satisfied when at least one of the Constraints is
 satisfied.

- [**odrl:or**](https://www.w3.org/TR/odrl-vocab/#term-or)


### [odrl:xone](http://www.w3.org/ns/odrl/2/xone)
"Only One": The relation is satisfied when only one, and not more, of the Constaints
 is satisfied. 

- [**odrl:xone**](https://www.w3.org/TR/odrl-vocab/#term-xone)

---