# 4.1 System Architecture


## 4.1.1 Frontend (User Interface)

The frontend of the application is built using HTML, CSS, and JavaScript with the following components:

- **Base Template**: Provides the common layout and navigation for all pages
  - Uses Bootstrap 5 for responsive design and styling
  - Includes navigation bar with conditional rendering based on authentication status
  - Handles flash messages for user notifications

- **User Authentication Pages**:.............................
  - Login page with username/password form
  - Registration page for new user accounts

- **Prediction Interface**:
  - Form for inputting house features (bathrooms, bedrooms, floor area, property type, etc.)
  - Client-side validation for required fields
  - JavaScript for asynchronous form submission via AJAX
  - Dynamic display of prediction results without page reload

- **Technologies Used**:
  - HTML5 for structure
  - Bootstrap 5 for styling and responsive design
  - JavaScript for client-side validation and AJAX requests
  - Jinja2 templating for server-side rendering

## 4.1.2 Backend (Web Server and API)

The backend is built using Flask, a lightweight Python web framework:

- **Web Server**: Flask application serving HTML templates and handling HTTP requests
  - Routes for user authentication (register, login, logout)
  - Routes for serving prediction form and processing predictions
  - Session management for user authentication state

- **API Endpoints**:
  - `/predict`: REST API endpoint accepting JSON input and returning price predictions
  - Form-based prediction endpoint for browser-based submissions

- **Authentication System**:
  - User registration with password hashing (PBKDF2 with SHA-256)
  - Login/logout functionality with session management
  - Route protection using login_required decorator

- **Technologies Used**:
  - Flask web framework
  - Werkzeug for security features (password hashing)
  - SQLite for database storage
  - Pickle for model serialization/deserialization

## 4.1.3 Machine Learning Model

The application uses an ensemble of machine learning models for house price prediction:

- **Model Architecture**:
  - Ensemble of CatBoost and LightGBM models
  - Weighted blending of predictions (60% CatBoost, 40% LightGBM)
  - Multiple models of each type for improved robustness and accuracy

- **Model Deployment**:
  - Models loaded at application startup
  - Predictions made in real-time based on user input
  - Log-transformation of predictions (np.expm1) to handle skewed price distributions

- **Technologies Used**:
  - CatBoost: Gradient boosting library with native support for categorical features
  - LightGBM: High-performance gradient boosting framework
  - NumPy for numerical operations
  - Pickle for model serialization

## 4.1.4 Data Preprocessing

The application includes preprocessing steps to prepare input data for the model:

- **Feature Handling**:
  - Automatic handling of missing features (setting defaults)
  - Proper typing of categorical features
  - Feature selection based on trained model requirements

- **Input Validation**:
  - Client-side validation for required fields and data types
  - Server-side preprocessing to ensure data compatibility with the model

- **Feature Engineering**:
  - Conversion of input data to the format expected by the model
  - Categorical feature encoding

- **Technologies Used**:
  - Pandas for data manipulation
  - Custom preprocessing functions

## 4.1.5 Model Training (Separate Script)

While not included in the web application itself, the model training process would have involved:

- **Data Collection and Cleaning**:
  - Gathering house price data with relevant features
  - Cleaning and preprocessing the dataset

- **Feature Engineering**:
  - Creating relevant features for house price prediction
  - Encoding categorical variables
  - Handling missing values

- **Model Selection and Training**:
  - Training multiple CatBoost and LightGBM models
  - Hyperparameter tuning
  - Cross-validation for model evaluation

- **Model Serialization**:
  - Saving trained models using pickle
  - Saving feature lists and categorical feature information

- **Technologies Used**:
  - Pandas and NumPy for data manipulation
  - CatBoost and LightGBM for model training
  - Scikit-learn for evaluation metrics and preprocessing
  - Pickle for model serialization

## 4.1.6 Database (Optional)

The application uses a simple SQLite database for user management:

- **Database Schema**:
  - Users table with id, username, and hashed password
  - Extensible design for future features

- **Database Operations**:
  - User creation during registration
  - User authentication during login
  - Database initialization at application startup

- **Security Features**:
  - Password hashing using PBKDF2 with SHA-256
  - Unique constraint on usernames

- **Technologies Used**:
  - SQLite for lightweight, file-based database
  - sqlite3 Python library for database operations