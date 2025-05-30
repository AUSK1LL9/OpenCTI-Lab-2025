# OpenCTI-Lab-2025
This repository contains a Docker Compose file for deploying the OpenCTI platform along with some free Open Source CTI feeds.

![image](https://github.com/user-attachments/assets/de603e6b-9c15-48ae-8b67-2ced9349c591)


## **Introduction**
OpenCTI is an open source platform allowing organizations to manage their cyber threat intelligence knowledge and observables. It has been created in order to structure, store, organize and visualize technical and non-technical information about cyber threats.

The structuration of the data is performed using a knowledge schema based on the STIX2 standards. It has been designed as a modern web application including a GraphQL API and an UX oriented frontend. Also, OpenCTI can be integrated with other tools and applications such as MISP, TheHive, MITRE ATT&CK, etc.

## **Installation**

To install Project Title, follow these steps:

1. Install RHEL/Rocky/Alama Linux (System Hardening guide can be found here *link here*)
2. Install Portainer (Optional - Portainer install guide can be found here *link here*)
3. Create a new Stack in Portainer Called OpenCTI
4. Load in OPenCTI build file from this repo
5. Set System variables (loaded in from redacted .yml file in this repo)
6. Build stack in Portainer
7. Log in and enjoy :)

![image](https://github.com/user-attachments/assets/b52b2017-9053-4cee-8c31-e9d96b6f497a)

## **Usage**

To use Project Title, follow these steps:

1. Access OpenCTI using a Web Browser on port 8080 (default - can be changed in docker file)
2. Optional: Ensure to enable MFA inside OpenCTI
3. Optional: Ensure to use Reverse Proxy to present Custom SSL certificate for authetntic, signed and verified HTTPS access

## **Contributing** (Share is caring)

If you'd like to contribute to Project Title, here are some guidelines:

1. Fork the repository.
2. Create a new branch for your changes.
3. Make your changes.
4. Write tests to cover your changes.
5. Run the tests to ensure they pass.
6. Commit your changes.
7. Push your changes to your forked repository.
8. Submit a pull request.

## **License**

Project Title is released under the MIT License. See the **[LICENSE](https://www.blackbox.ai/share/LICENSE)** file for details.

## **Authors and Acknowledgment**

Project Title was created by **[Your Name](https://github.com/username)**.

Additional contributors include:

- **[Contributor Name](https://github.com/contributor-name)**
- **[Another Contributor](https://github.com/another-contributor)**

Thank you to all the contributors for their hard work and dedication to the project.

## **Code of Conduct**

Please note that this project is released with a Contributor Code of Conduct. By participating in this project, you agree to abide by its terms. See the **[CODE_OF_CONDUCT.md](https://www.blackbox.ai/share/CODE_OF_CONDUCT.md)** file for more information.

## **FAQ**

**Q:** What is Project Title?

**A:** Project Title is a project that does something useful.

**Q:** How do I install Project Title?

**A:** Follow the installation steps in the README file.

**Q:** How do I use Project Title?

**A:** Follow the usage steps in the README file.

**Q:** How do I contribute to Project Title?

**A:** Follow the contributing guidelines in the README file.

**Q:** What license is Project Title released under?

**A:** Project Title is released under the MIT License. See the **[LICENSE](https://www.blackbox.ai/share/LICENSE)** file for details.

## **Changelog**

- **0.1.0:** Initial release
- **0.1.1:** Fixed a bug in the build process
- **0.2.0:** Added a new feature
- **0.2.1:** Fixed a bug in the new feature

## **Contact**

If you have any questions or comments about Project Title, please contact **[Your Name](you@example.com)**.

## **Conclusion**

That's it! This is a basic template for a proper README file for a general project. You can customize it to fit your needs, but make sure to include all the necessary information. A good README file can help users understand and use your project, and it can also help attract contributors.
