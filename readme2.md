// index.js
const express = require('express');
const dotenv = require('dotenv');
const logger = require('./middleware/logger');
const userRoutes = require('./routes/userRoutes');
const productRoutes = require('./routes/productRoutes');

// Load environment variables
dotenv.config();

const app = express();
const PORT = process.env.PORT || 5000;

// Middleware
app.use(express.json());
app.use(logger);

// Routes
app.use('/api/users', userRoutes);
app.use('/api/products', productRoutes);

// Global Error Handling
app.use((err, req, res, next) => {
    console.error(err.stack);
    res.status(500).json({ message: 'Internal Server Error' });
});

// Start Server
app.listen(PORT, () => {
    console.log(`Server running on port ${PORT}`);
});

// middleware/logger.js
module.exports = (req, res, next) => {
    console.log(`${req.method} ${req.url} - ${new Date().toISOString()}`);
    next();
};

// routes/userRoutes.js
const express = require('express');
const { getUsers, createUser } = require('../controllers/userController');
const router = express.Router();

router.get('/', getUsers);
router.post('/', createUser);

module.exports = router;

// routes/productRoutes.js
const express = require('express');
const { getProducts, createProduct } = require('../controllers/productController');
const router = express.Router();

router.get('/', getProducts);
router.post('/', createProduct);

module.exports = router;

// controllers/userController.js
exports.getUsers = (req, res) => {
    res.json({ message: 'Get all users' });
};

exports.createUser = (req, res) => {
    res.json({ message: 'Create a user' });
};

// controllers/productController.js
exports.getProducts = (req, res) => {
    res.json({ message: 'Get all products' });
};

exports.createProduct = (req, res) => {
    res.json({ message: 'Create a product' });
};