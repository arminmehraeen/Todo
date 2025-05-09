# Modern Todo Application

A modern, responsive Todo application built with Vue 3 and Bootstrap 5. This application allows users to manage their tasks with a beautiful and intuitive interface.

## Features

- Create, read, update, and delete tasks
- Task prioritization (Low, Medium, High)
- Color coding for tasks
- Task status tracking (Active/Completed)
- Responsive design
- Modern UI with animations
- Form validation
- Sweet alerts for user feedback

## Technologies Used

- Vue 3
- Bootstrap 5
- SweetAlert2
- Animate.css
- SASS

## Project Setup

```bash
# Install dependencies
npm install

# Serve with hot reload at localhost:8080
npm run serve

# Build for production
npm run build

# Lint and fix files
npm run lint
```

## Deployment

The application is configured for deployment to GitHub Pages. To deploy:

1. Update the `deploy.sh` script with your GitHub username
2. Run the deployment script:
```bash
npm run deploy
```

## Project Structure

```
todo/
├── public/
├── src/
│   ├── components/
│   │   ├── Header.vue
│   │   ├── Footer.vue
│   │   └── Card.vue
│   ├── App.vue
│   └── main.js
├── package.json
└── README.md
```

## Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- Vue.js team for the amazing framework
- Bootstrap team for the UI components
- SweetAlert2 for beautiful alerts
