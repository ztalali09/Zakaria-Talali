.projects-section {
  position: relative;
  width: 100%;
  min-height: 100vh;
  background: #FFFFFF;
  border-radius: 2.5rem 2.5rem 0 0;
  margin-top: -2rem;
  overflow: hidden;
}

.section-title {
  font-size: 1rem;
  font-weight: 300;
  color: #000000;
  letter-spacing: 0.1em;
}

.section-number {
  color: #000000;
}

.border-bottom {
  width: 100%;
  height: 1px;
  background-color: rgba(0, 0, 0, 0.2);
  margin-top: 1rem;
}

.main-heading {
  font-size: clamp(2.25rem, 5vw, 4.5rem);
  font-weight: 400;
  color: #000000;
  line-height: 1.15;
  letter-spacing: -0.02em;
}

.project-item {
  padding: 3rem;
  border-top: 1px solid rgba(0, 0, 0, 0.2);
  cursor: pointer;
  transition: all 0.3s ease;
  position: relative;
  z-index: 1;
  display: flex;
  flex-direction: column;
  justify-content: center;
  min-height: 200px;
  background-color: transparent;
}

.project-item:last-child {
  border-bottom: 1px solid rgba(0, 0, 0, 0.2);
}

.project-item:hover {
  padding-left: 4rem;
  background-color: #000000;
}

.project-title {
  font-size: clamp(1.5rem, 4vw, 2.25rem);
  font-weight: 300;
  color: #000000;
  margin-bottom: 1rem;
  transition: all 0.3s ease;
}

.project-item:hover .project-title {
  color: #FFFFFF;
}

.project-description {
  font-size: 1.125rem;
  color: rgba(0, 0, 0, 0.8);
  transition: all 0.3s ease;
}

.project-item:hover .project-description {
  color: #FFFFFF;
}

.custom-cursor {
  position: fixed;
  width: 20px;
  height: 20px;
  background-color: #000000;
  border-radius: 50%;
  pointer-events: none;
  opacity: 0;
  transition: opacity 0.3s ease;
  transform: translate(-50%, -50%);
}

.cursor-label {
  position: fixed;
  color: #FFFFFF;
  font-size: 14px;
  pointer-events: none;
  opacity: 0;
  transition: opacity 0.3s ease;
  transform: translate(-50%, -50%);
  background-color: #000000;
  padding: 4px 8px;
  border-radius: 4px;
  white-space: nowrap;
}

.preview-image {
  max-height: 100%;
  max-width: 100%;
  width: auto;
  height: auto;
  object-fit: contain;
  border-radius: 4px;
  transform-origin: center center;
  will-change: transform;
  filter: drop-shadow(0 4px 6px rgba(0, 0, 0, 0.1));
} 