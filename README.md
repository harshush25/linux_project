#!/bin/bash

# Burger Shop Management System

# Initialize an array of burger options and their prices
BURGERS=("Cheese burger" "Mac veggie" "Signature Burger" "aloo tikki Burger" "Double Bacon Burger")
PRICES=(3.50 4.00 3.75 3.25 5.00)

# Arrays to store orders and total sales
ORDERS=()
TOTAL_SALES=0

# Function to display the menu
display_menu() {
    echo "=============================="
    echo "      Welcome to the Burger Shop!"
    echo "=============================="
    echo "Available Burgers:"
    for i in "${!BURGERS[@]}"; do
        echo "$((i + 1)). ${BURGERS[i]} - \$${PRICES[i]}"
    done
    echo "0. Exit"
    echo "=============================="
}

# Function to take an order from the user
take_order() {
    read -p "Please enter the number of the burger you want to order: " choice
    if [[ $choice -ge 1 && $choice -le ${#BURGERS[@]} ]]; then
        # Add the selected burger to the order list
        ORDERS+=("${BURGERS[choice - 1]}")
        # Update the total sales
        TOTAL_SALES=$(echo "$TOTAL_SALES + ${PRICES[choice - 1]}" | bc)
        echo "Thank you! You ordered: ${BURGERS[choice - 1]}"
    elif [[ $choice -eq 0 ]]; then
        echo "Thank you for visiting! Exiting..."
    else
        echo "Invalid choice. Please try again."
    fi
}

# Function to display the order summary
display_summary() {
    echo -e "\n=============================="
    echo "          Order Summary"
    echo "=============================="
    if [ ${#ORDERS[@]} -eq 0 ]; then
        echo "No orders placed."
    else
        for burger in "${ORDERS[@]}"; do
            echo "- $burger"
        done
        echo "=============================="
        echo "Total Sales: \$$(printf "%.2f" $TOTAL_SALES)"
    fi
    echo "Thank you for your order!"
}

# Main loop to run the burger shop
while true; do
    display_menu
    take_order
    # Break the loop if the user chooses to exit
    if [[ $? -eq 0 ]]; then
        break
    fi
done

# Display the order summary when exiting
display_summary 
