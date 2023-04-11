# HW-6-Bobcat-Auto-Dealership-
#include <iostream>
#include <fstream>
#include <string>
#include <vector>
#include <algorithm>
#include <iomanip>

using namespace std;

const double E_PRICE = 10000.0;
const double L_PRICE = 12000.0;
const double X_PRICE = 18000.0;

const int MAX_OPTIONS = 6;

vector<string> options;
vector<double> prices;
vector<string> selected_options;

void read_options() {
    ifstream fin("options.txt");
    if (!fin) {
        cerr << "Cannot open options.txt" << endl;
        exit(1);
    }
    string option;
    double price;
    while (fin >> option >> price) {
        options.push_back(option);
        prices.push_back(price);
    }
    fin.close();
}

void display_options() {
    cout << "Available Options" << endl;
    for (int i = 0; i < options.size(); i++) {
        cout << left << setw(20) << options[i] << right << "$" << setw(7) << fixed << setprecision(2) << prices[i] << " ";
        if ((i + 1) % 3 == 0) {
            cout << endl;
        }
    }
    if (options.size() % 3 != 0) {
        cout << endl;
    }
}

void select_model(char& model) {
    while (true) {
        cout << "Enter the model (E, L, X): ";
        cin >> model;
        if (model == 'e' || model == 'E' || model == 'l' || model == 'L' || model == 'x' || model == 'X') {
            break;
        }
        cout << "Invalid model. Please try again." << endl;
    }
    switch (model) {
    case 'e':
    case 'E':
        cout << "Model: E, $" << fixed << setprecision(2) << E_PRICE << ", Options: None" << endl;
        break;
    case 'l':
    case 'L':
        cout << "Model: L, $" << fixed << setprecision(2) << L_PRICE << ", Options: None" << endl;
        break;
    case 'x':
    case 'X':
        cout << "Model: X, $" << fixed << setprecision(2) << X_PRICE << ", Options: None" << endl;
        break;
    }
}

bool is_valid_option(string option) {
    return find(options.begin(), options.end(), option) != options.end();
}

bool is_duplicate_option(string option) {
    return find(selected_options.begin(), selected_options.end(), option) != selected_options.end();
}

void add_option() {
    if (selected_options.size() >= MAX_OPTIONS) {
        cout << "Cannot add more than " << MAX_OPTIONS << " options." << endl;
        return;
    }
    string option;
    while (true) {
        cout << "Enter option (or 'cancel' to cancel): ";
        cin >> option;
        if (option == "cancel") {
            return;
        }
        if (is_valid_option(option)) {
            if (is_duplicate_option(option)) {
                cout << "Option already selected." << endl;
                continue;
            }
            int index = find(options.begin(), options.end(), option) - options.begin();
            selected_options.push_back(option);
            cout << "Model: ";
            switch (tolower(option[0])) {
            case 'l':
                cout << "L";
                break;
            case 'x':
                cout << "X";
                break;
